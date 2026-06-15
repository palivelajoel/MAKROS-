# MAKROS | μακρός

<div align="center">

![MAKROS Banner](docs/images/banner.png)

**A compact, open-source 4×3 macro pad with rotary encoder, OLED display, and per-key RGB.**

---

[![License: MIT](https://img.shields.io/badge/License-MIT-black?style=flat-square)](LICENSE)
[![KiCad](https://img.shields.io/badge/PCB-KiCad-black?style=flat-square)](hardware/pcb)
[![RP2040](https://img.shields.io/badge/MCU-RP2040-black?style=flat-square)](firmware)
[![CircuitPython](https://img.shields.io/badge/Firmware-CircuitPython-black?style=flat-square)](firmware)
[![Status: WIP](https://img.shields.io/badge/Status-Work%20In%20Progress-black?style=flat-square)]()

</div>

---

## Overview

MAKROS (μακρός — Greek for "long" or "large") is a fully open-source macro pad designed for productivity, streaming, and desktop automation. It features a 4×3 switch matrix, a rotary encoder, a small OLED display, and per-key RGB underglow — all driven by the Seeed XIAO RP2040.

All design files — CAD, schematics, PCB layouts, and firmware — are publicly available in this repository.

---

## Components Used

| Component | Specification | Qty |
|---|---|---|
| Seeed XIAO RP2040 | RP2040 MCU, USB-C, castellated pads | 1 |
| MX-style mechanical switches | Any MX-compatible switch | 12 |
| EC11 rotary encoder | 20mm D-shaft, with push-button | 1 |
| 0.91" OLED display | 128x32, I2C, 3.3V | 1 |
| 1N4148 diodes | Through-hole, DO-35 | 12 |
| SK6812 MINI-E | Reverse-mount RGB LED | 12 |
| Blank DSA keycaps | 1u | 12 |
| M3x16mm screws | Button or socket head | 6 |
| M3x5x4mm heatset inserts | For M3 screws | 6 |

> Full purchase links and supplier pages are listed in [`RESOURCES.md`](RESOURCES.md).

---

## Table of Contents

- [Features](#features)
- [Hardware](#hardware)
  - [Bill of Materials](#bill-of-materials)
  - [PCB](#pcb)
  - [Schematic](#schematic)
  - [CAD / Enclosure](#cad--enclosure)
- [Firmware](#firmware)
- [Build Guide](#build-guide)
- [Gallery](#gallery)
- [License](#license)

---

## Features

- 12 MX-compatible mechanical switches in a 4×3 grid
- 1× EC11 rotary encoder with push-button
- 0.91" 128×32 OLED display for layer/mode indication
- Per-key SK6812 MINI-E RGB underglow
- Seeed XIAO RP2040 microcontroller (USB-C)
- Custom 3D-printed enclosure with top plate
- 100×100mm PCB (JLCPCB / PCBWay compatible)
- Open-source firmware, PCB, schematic, and CAD

---

## Hardware

### Bill of Materials

| Part | Value / Spec | Qty |
|---|---|---|
| Seeed XIAO RP2040 | Microcontroller | 1 |
| MX-style mechanical switches | Any MX-compatible | 12 |
| EC11 rotary encoder | 20mm D-shaft | 1 |
| 0.91" OLED display | 128×32, I2C | 1 |
| 1N4148 diodes | Through-hole | 12 |
| SK6812 MINI-E | RGB LED, reverse mount | 12 |
| Blank DSA keycaps | 1u | 12 |
| M3×16mm screws | — | 6 |
| M3×5×4mm heatset inserts | — | 6 |

---

### PCB

The PCB is designed in KiCad and fits within a **100×100mm** footprint for low-cost manufacturing. Diodes are mounted on the underside of the board to maintain clearance.

> **Place your PCB front render here**
> `docs/images/pcb_front.png`

> **Place your PCB back render here**
> `docs/images/pcb_back.png`

**Key design decisions:**
- Through-hole diodes flipped to the bottom side to keep the board within 100×100mm
- XIAO RP2040 mounted via castellated pads / pin headers
- SK6812 MINI-E LEDs use reverse-mount footprints for under-switch RGB
- I2C routed to OLED header in top-left corner
- Encoder footprint in top-left of switch grid

**Manufacturing files** (Gerbers, BOM, CPL) are located in [`hardware/pcb/fab/`](hardware/pcb/fab/).

---

### Schematic

> **Place your schematic image here**
> `docs/images/schematic.png`

The full schematic is available as a KiCad project in [`hardware/schematic/`](hardware/schematic/) and as an exported PDF in [`docs/schematic.pdf`](docs/schematic.pdf).

**Key nets:**
- Switch matrix: 4 columns × 3 rows, diode per key
- OLED: I2C via GP6/GP7
- Encoder A/B/SW: GP0, GP1, GP2
- RGB data chain: GP3 → SK6812 MINI-E ×12

---

### CAD / Enclosure

The enclosure is a two-part design: a bottom tray and a top switch plate. Both are designed for FDM printing.

> **Place your enclosure render here**
> `docs/images/cad_exploded.png`

All CAD files are available in [`hardware/cad/`](hardware/cad/) in the following formats:

| Format | File |
|---|---|
| Fusion 360 | `makros.f3d` |
| STEP | `makros.step` |
| STL (top plate) | `top_plate.stl` |
| STL (bottom tray) | `bottom_tray.stl` |

**Print settings (recommended):**

| Setting | Value |
|---|---|
| Material | PLA or PETG |
| Layer height | 0.2mm |
| Infill | 20% |
| Supports | Top plate: none / Tray: brim recommended |

---

## Firmware

> **Place a screenshot or photo of the OLED display in use here**
> `docs/images/firmware_oled.png`

Firmware is written in CircuitPython and located in [`firmware/`](firmware/).

```
firmware/
├── code.py           # Main entry point
├── keymap.py         # Layer and key definitions
├── oled.py           # Display handler
├── encoder.py        # Rotary encoder logic
└── rgb.py            # SK6812 LED control
```

**Features implemented:**
- Multiple layers (switchable via encoder press)
- OLED shows current layer name and mode
- Encoder controls volume / scroll / custom action per layer
- Per-key RGB with configurable colors per layer
- HID keyboard and media key support

**Flashing:**

1. Hold the BOOT button on the XIAO RP2040 while plugging in USB
2. A drive called `RPI-RP2` will appear
3. Copy [`firmware/adafruit-circuitpython-*.uf2`](firmware/) to the drive
4. Once rebooted, copy the contents of [`firmware/`](firmware/) to the `CIRCUITPY` drive

---

## Build Guide

> **Place a photo of the completed build here**
> `docs/images/build_complete.png`

**Tools required:** Soldering iron, solder, flush cutters, screwdriver (M3), heatset insert tip (optional)

**Steps:**

1. **Solder diodes** — Install 1N4148 diodes on the bottom side of the PCB, observing polarity (cathode to square pad).
2. **Solder SK6812 MINI-E LEDs** — Solder to the bottom-side LED pads. The notched corner marks GND.
3. **Solder XIAO RP2040** — Align to the footprint and solder all pads.
4. **Solder OLED header** — 4-pin header in the top-left position.
5. **Solder encoder** — Press-fit EC11 into footprint and solder all 5 pins and 2 support legs.
6. **Install heatset inserts** — Press M3×5×4mm inserts into the bottom tray using a soldering iron.
7. **Install switches** — Snap MX switches into the top plate, then lower onto the PCB and solder.
8. **Assemble enclosure** — Seat the PCB into the bottom tray, align the top plate, and secure with M3×16mm screws.
9. **Flash firmware** — Follow the [Firmware](#firmware) section above.
10. **Install keycaps** — Press DSA keycaps onto switch stems.

---

## Gallery

| | |
|---|---|
| ![CAD Exploded View](docs/images/cad_exploded.png) | ![PCB Front](docs/images/pcb_front.png) |
| CAD — Exploded view | PCB — Front |
| ![PCB Back](docs/images/pcb_back.png) | ![Completed Build](docs/images/build_complete.png) |
| PCB — Back | Completed build |

---

## Repository Structure

```
MAKROS/
├── hardware/
│   ├── pcb/            # KiCad PCB project + Gerbers
│   ├── schematic/      # KiCad schematic
│   └── cad/            # Fusion 360, STEP, STL files
├── firmware/           # CircuitPython source
├── docs/
│   ├── images/         # All images referenced in this README
│   └── schematic.pdf
├── LICENSE
└── README.md
```

---

## License

Hardware and CAD files are released under the [CERN Open Hardware Licence v2 - Permissive](LICENSE-HARDWARE).  
Firmware is released under the [MIT License](LICENSE-FIRMWARE).

---

<div align="center">

Designed by Joel Palivela

</div>
