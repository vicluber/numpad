# Project Context & Hardware Memory

This file serves as persistent context for AI assistants working on this repository.

---

## Hardware Configuration

* **Microcontroller:** SuperMini / ProMicro nRF52840 clone (model `V1940`).
* **Silkscreen Marking:** The board labels pins directly with raw nRF52840 GPIO identifier numbers (`017`, `020`, `031`, etc.) rather than standard Arduino / Pro Micro numbers.

---

## Physical Matrix Mapping & Tested Pins

### Verified Working Pins:
* **Columns (Right side in back view / Silk side up, USB at top):**
  * `Col 0` -> Silk `017` (`pro_micro 2` / `P0.17`) - Pin 6 from top
  * `Col 1` -> Silk `020` (`pro_micro 3` / `P0.20`) - Pin 7 from top
  * `Col 2` -> Silk `022` (`pro_micro 4` / `P0.22`) - Pin 8 from top
  * `Col 3` -> Silk `024` (`pro_micro 5` / `P0.24`) - Pin 9 from top
* **Rows (Left side in back view / Silk side up, USB at top):**
  * `Row 1` -> Silk `029` (`pro_micro 20` / `P0.29`) -> Keys `7`, `8`, `9`, `+` - Pin 6 from top
  * `Row 2` -> Silk `002` (`pro_micro 19` / `P0.02`) -> Keys `4`, `5`, `6` - Pin 7 from top
  * `Row 3` -> Silk `111` (`pro_micro 14` / `P1.11`) -> Keys `1`, `2`, `3`, `Enter` - Pin 10 from top (anteriormente 115)
  * `Row 4` -> Silk `010` (`pro_micro 16` / `P0.10`) -> Keys `0`, `.` - Pin 11 from top (anteriormente 113)

---

## Issue: Suspected Damaged Pin `031` (Row 0)

* **Problem:** Physical pin `031` (`pro_micro 21` / `P0.31` / Row 0) was tested and worked before soldering, but stopped giving output after soldering (likely trace/pad damage from heat).
* **Requirement:** Keep `031` active in firmware just in case, but mirror (clone) Row 0 behavior to unused pins so the user can solder the first row wire to a working alternative pin without remapping key bindings manually.

---

## Implemented Solution (Matrix Row Cloning)

In `boards/shields/generic-numpad/generic-numpad.dtsi` and `config/generic-numpad.keymap`:
1. Matrix scan expanded from 5 to 7 rows (`rows = <7>`).
2. Added two backup row pins to `kscan0` row-gpios:
   * **Backup 1:** Silk `100` (`pro_micro 6` / `P1.00`) -> Right side, Pin 10 (directly below Col 3 `024`).
   * **Backup 2:** Silk `009` (`pro_micro 10` / `P0.09`) -> Left side, Pin 12 (bottom pin on the left).
3. Cloned the exact Row 0 key bindings (`&mo 1`, `KP_DIVIDE`, `KP_MULTIPLY`, `KP_MINUS`) to Row 5 and Row 6 on all keymap layers.

---

## Flashing & Bluetooth Reset Notes

* Added `settings_reset` shield for `nice_nano_v2` in `build.yaml` to generate `settings_reset-nice_nano_v2-zmk.uf2`.
* Flashing `settings_reset.uf2` wipes all corrupted Bluetooth bonding data from flash memory if pairing fails.
