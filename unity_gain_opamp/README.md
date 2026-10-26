# N-Pulse Workshop Board

A teaching board for the N-Pulse electronics workshop: an ESP32-S3 platform with
a complete battery power path, four sensors, and a heart-rate (PPG) front end —
designed so that new members can learn hardware design by reading a real,
working board rather than a toy example.

![Top view of the N-Pulse workshop board](docs/board-top.webp)

## What this board is for

Most beginner projects are one circuit. This board is deliberately the opposite:
it puts one worked example of nearly every subsystem you meet in real hardware
onto a single PCB, so that "how do I charge a battery?", "how do I protect a USB
port?" or "how do I amplify a microvolt sensor?" all have an answer you can hold
in your hand and probe with a scope.

It is a **reference**, not a first project. Read it, measure it, copy from it.

Every design decision is documented in **[DESIGN_NOTES.md](DESIGN_NOTES.md)**,
which also contains general guidance on how to draw a good schematic and how to
lay out a good PCB.

## Features

**Power**
- USB-C input with ESD protection and a TVS on VBUS
- MCP73833 Li-ion charger (1 A) with charge / done / power-good indicators
- BQ29728 cell protection with back-to-back MOSFETs
- TPS631000 buck-boost — holds 3.3 V across the whole 4.2 → 3.0 V cell range
- TPS2116 priority mux — runs from USB when present, battery otherwise
- TPS2068 load switch — firmware can cut power to the peripherals
- Two rails: `+3V3_ESP32` (always on) and `+3V3` (switched)

**Sensing**
- SHT40 — humidity and temperature (I²C)
- LIS2DW12 — 3-axis accelerometer with interrupts, on the always-on rail so the
  board can wake on motion
- MCP9700A — analog temperature
- PPG pulse sensor — LED + photodiode into a two-stage bandpass amplifier
  (0.34–5 Hz, gain ×121)

**Interfaces**
- ESP32-S3-WROOM-1, programmed over native USB — no bridge chip, no drivers
- microSD card slot with mechanical card detect
- Two RGB LEDs and a piezo buzzer
- I²C expansion header
- 17 labelled test points

**Physical:** 70 × 80 mm, 2-layer, 142 components, four M4 mounting holes.

## Using the board

**Powering it.** Plug in USB-C and it runs, regardless of the ON/OFF switch. On
battery, slide the ON/OFF switch (bottom edge, marked `ON`/`OFF`). With USB
connected the board runs from USB and charges the cell at the same time.

**Charging.** The three LEDs on the left edge are, top to bottom, `CHRG`
(charging), `DONE` (complete) and `PG` (USB power good).

**Programming.** Connect USB-C — the board enumerates directly as an ESP32-S3.
Normally you can just flash it. If you need to force download mode: slide the
`BOOT0` switch (right edge) to the boot position, press `ENABLE` to reset, flash,
then slide it back and reset again. The LED beside the switch shows its position.

**Reading your pulse.** Rest a fingertip on the fingerprint symbol in the middle
of the board, covering the LED and photodiode, with light steady pressure. The
amplifier needs a second or two to settle. Signal quality depends a lot on
contact pressure and ambient light — that is part of what makes it a good
teaching circuit.

**I²C expansion.** The 4-pin header on the right edge is `3V3 / SDA / SCL / GND`
(labelled on the silkscreen). It is ESD-protected and shares the bus with the
on-board sensors, so remember the address space is shared.

**Test points.** Every rail and every interesting node is brought out:
`+5V USB`, `3V3 USB`, `3V3 BATT`, `3V3 ESP32`, `3V3`, `+BATT`, `-BATT`, `GND`,
plus `FB`, `PR1`, `MODE` and `PWR SW` for the power circuits.

## Opening the project

Built with **KiCad 9/10**. Clone and open `Electronics_Workwhop_PCB.kicad_pro`.

All custom symbols and footprints live in the repository — `N_pulse.pretty/` for
footprints and `Library/` for vendor symbol libraries — so the project should
open with no missing parts. Nothing needs to be installed globally.

```
Electronics_Workwhop_PCB.kicad_pro   project file — open this
├── Architecture.kicad_sch           top-level sheet
├── ESP32.kicad_sch                  MCU, SD card, LEDs, buzzer, buttons
├── Power_management.kicad_sch       USB-C, charger, protection, regulators
├── Sensors.kicad_sch                SHT40, accelerometer, temperature, PPG
└── Electronics_Workwhop_PCB.kicad_pcb
DESIGN_NOTES.md                      why the circuits look the way they do
docs/                                images and documentation assets
```

## Pin map

See [Appendix A of DESIGN_NOTES.md](DESIGN_NOTES.md) for the full table,
generated from the netlist.

The one rule worth memorising: **analog inputs must be on ADC1 (IO1–IO10)**.
ADC2 cannot be read while Wi-Fi is running.

## Contributing

KiCad files cannot be merged. If two people edit the same sheet at the same
time, git produces a conflict that no one can resolve by hand.

Before you start work, say which sheet you are taking, and pull before you open
the project. Commit with a message describing the change, not just "update".

The pre-fabrication checklist in
[Appendix B of DESIGN_NOTES.md](DESIGN_NOTES.md) should be run before any board
is ordered.

---

> *"What I cannot create, I do not understand."* — Richard Feynman
>
> (also printed on the board)
