# Numpad Matrix & Wiring Guide

Guide for hand-wiring a traditional 17-key mechanical numpad using a nice!nano (or SuperMini nRF52840 clone).

---

## 1. Matrix Overview

* **Matrix Dimensions:** 5 Rows × 4 Columns
* **Total Switches:** 17 switches
* **Diodes Needed:** 17 × 1N4148 signal diodes
* **GPIO Pins Required:** 9 pins (5 for rows + 4 for columns)

---

## 2. Layout & Matrix Mapping (5 × 4)

Standard traditional numpad layout:

| Row / Col | Col 0 | Col 1 | Col 2 | Col 3 |
| :--- | :--- | :--- | :--- | :--- |
| **Row 0** | `Num Lock` | `/` | `*` | `-` |
| **Row 1** | `7` | `8` | `9` | `+` (2U switch) |
| **Row 2** | `4` | `5` | `6` | *(empty / bridged)* |
| **Row 3** | `1` | `2` | `3` | `Enter` (2U switch) |
| **Row 4** | `0` (2U switch) | *(empty / bridged)* | `.` | *(empty / bridged)* |

> **Note on 2U Keys (`0`, `+`, `Enter`):**  
> Each 2U key uses only a single switch with stabilizers. In the matrix, they map to one specific coordinate (e.g., `+` on Row 1 / Col 3, `Enter` on Row 3 / Col 3, and `0` on Row 4 / Col 0).

---

## 3. Wiring Instructions

### A. Diodes & Rows (Horizontal)
1. Solder one **1N4148 diode** to one pin of every switch.
2. **Diode Orientation (`COL2ROW` standard):**
   * Solder the **anode** (unmarked side) to the switch pin.
   * Point the **cathode** (black line side) away from the switch.
3. Join all diode cathodes horizontally in the same row using insulated or bus wire to form **5 Row lines** (Row 0 through Row 4).

### B. Columns (Vertical)
1. Solder insulated wire connecting the remaining free pin of each switch vertically.
2. Join them column-by-column to form **4 Column lines** (Col 0 through Col 3).

---

## 4. nice!nano / SuperMini nRF52840 Pin Assignment (Generic-numpad Mapping)

Connect the 9 lines to the specific GPIO pins defined in [Sadteeto/Generic-numpad](https://github.com/Sadteeto/Generic-numpad):

| Matrix Line | Board Silkscreen Pin | nRF52840 GPIO (ZMK) | Physical Board Side |
| :--- | :--- | :--- | :--- |
| **Row 0** | `21` (or `A0`) | `P0.31` | Right Side |
| **Row 1** | `20` (or `A1`) | `P0.29` | Right Side |
| **Row 2** | `19` (or `A2`) | `P0.02` | Right Side |
| **Row 3** | `18` (or `A3`) | `P1.15` | Right Side |
| **Row 4** | `15` (or `SCK`) | `P1.13` | Right Side |
| **Col 0** | `2` (or `D2`) | `P0.17` | Left Side |
| **Col 1** | `3` (or `D3`) | `P0.20` | Left Side |
| **Col 2** | `4` (or `D4`) | `P0.22` | Left Side |
| **Col 3** | `5` (or `D5`) | `P0.24` | Left Side |

---

## 5. Firmware Build & Flashing (via GitHub Actions)

This wiring is 100% plug-and-play compatible with the [Sadteeto/Generic-numpad](https://github.com/Sadteeto/Generic-numpad) repository:

1. **Fork Repository:** Go to [Sadteeto/Generic-numpad](https://github.com/Sadteeto/Generic-numpad) and click **Fork**.
2. **Enable Workflows:** In your forked repository, go to **Actions** and click **"I understand my workflows, go ahead and enable them"**.
3. **Trigger Build:** Select the **Build** workflow on the left, click **Run workflow** -> **Run workflow**.
4. **Download Firmware:** When completed (~1-2 min), open the workflow run and download the `.zip` from **Artifacts**.
5. **Flash Microcontroller:**
   * Double-tap the **Reset** pin/button on your nice!nano / SuperMini board to enter bootloader mode.
   * A USB drive named `NICENANO` will mount on your computer.
   * Drag and drop the `.uf2` file into the `NICENANO` drive. The board will automatically flash and reboot.

