# Touchscreen-Based Multichannel Scaler Discriminator Module

A touchscreen-operated pulse-counting system developed during a summer project at the Bhabha Atomic Research Centre (BARC), focused on embedded instrumentation for neutron pulse-counting applications.


## Overview

The scaler receives conditioned detector pulses, counts them over a configurable time interval, and displays the result through a touchscreen graphical interface. The project was implemented on two embedded platforms while maintaining the core scaler functions.

## Main Features

- Start, Stop, and Reset controls for pulse counting
- User-configurable counting time and scale factor
- Count and count-rate display
- Resistive-touchscreen implementation on Arduino Mega 2560
- Port to the WT32-SC01 ESP32 board with capacitive touchscreen
- Real-time count-rate graph on WT32-SC01
- Five-sample moving-average filter for a steadier displayed count rate on WT32-SC01
- Separate FreeRTOS task for graph updates on the WT32-SC01
- Analog pulse-conditioning front end and four-layer PCB design using KiCad

## System Architecture

```text
Detector / Test Pulse Input
           |
           v
 Analog Front End
 (LM358 signal conditioning,
  LM319 comparator / discrimination)
           |
           v
 Digital Pulse Signal
           |
           v
 Microcontroller Pulse Counting
           |
           +-----------------------------+
           |                             |
           v                             v
 Count / Rate Calculation        Time + Scale Settings
           |                             |
           +--------------+--------------+
                          |
                          v
                 Touchscreen GUI
             Count, Rate, Controls
                          |
                          v
              Optional Real-Time Graph
                  (WT32-SC01)
```

*The diagram is a high-level functional representation; consult the actual circuit and firmware files before building hardware.*

## Hardware

### Platform A — Arduino Mega 2560

- Arduino Mega 2560 microcontroller board
- 2.4-inch TFT resistive touchscreen shield
- MCUFRIEND_kbv, Adafruit_GFX, and TouchScreen libraries
- Pin mapping was modified in the `MCUFRIEND_kbv` configuration to suit the project setup while preserving touchscreen operation.

### Platform B — WT32-SC01

- ESP32-WROVER-B-based WT32-SC01 board
- Integrated 3.5-inch capacitive touchscreen (320 × 480)
- LovyanGFX and LVGL libraries, as described in the project report
- FreeRTOS task used to separate graph rendering from the main interface/counting loop

### Analog Front End and PCB

- **LM358:** signal conditioning / buffering, depending on circuit configuration
- **LM319:** comparator for pulse discrimination and conversion to a digital pulse
- **LM7805:** regulated +5 V supply
- **KiCad:** schematic capture and four-layer PCB layout

The final public repository should include the verified schematic, component values, connector/pin definitions, and PCB manufacturing outputs only after these have been checked and approved for release.

## Software Behaviour

1. The user opens the main screen.
2. The user chooses a counting duration and scale factor in Setup.
3. Start begins pulse counting and updates the displayed count and rate.
4. Stop halts counting; Reset clears the count and related displayed values.
5. On WT32-SC01, the count-rate display is smoothed using a five-sample moving average.
6. A dedicated FreeRTOS task updates the real-time graph independently of the main application loop.

## Development Tools and Libraries

| Area | Tools / Libraries |
|---|---|
| Arduino platform | Arduino IDE, MCUFRIEND_kbv, Adafruit_GFX, TouchScreen |
| WT32-SC01 platform | Arduino IDE / ESP32 toolchain, LovyanGFX, LVGL, FreeRTOS task APIs |
| PCB design | KiCad |
| Analog front end | LM319 comparator, LM358 op-amp, LM7805 regulator |

## Repository Structure

```text
.
├── README.md
├── CONTRIBUTING.md
├── .gitignore
├── docs/
│   └── (public project notes and approved images)
├── firmware/
│   ├── arduino-mega/
│   └── wt32-sc01/
├── hardware/
│   ├── schematic/
│   └── pcb/
└── media/
    └── (approved setup photos and diagrams)
```

Directories for which source files have not yet been added are planned locations, not claims that the corresponding files are already included.

## Validation

The project report documents bench testing with generated test pulses. For example, it describes a 2 kHz input and a displayed count of 2000 over 10 seconds on the Arduino Mega setup, and a 1 kHz input with a displayed count of 1000 over 10 seconds on the WT32-SC01 setup. These are report examples, not a substitute for independent reproduction or a formal performance specification.

## Limitations and Safety

- This is an educational/research instrumentation project, not a certified safety or radiation-monitoring instrument.
- Do not connect a detector or external signal source until voltage levels, grounding, input protection, and pulse specifications have been verified against the actual circuit.
- The repository should not be used as a build guide until firmware, pin mappings, component values, and the analog front-end schematic are included and checked.
- Confirm institutional/project permissions before publishing any BARC-associated circuit, photograph, document, or other material.

## Credits

Developed as a summer project at the Bhabha Atomic Research Centre (BARC), June–July 2026, as documented in the project report. Institutional names are used only to describe the project context; this repository is not an official BARC publication.

## References

- [Arduino Mega 2560](https://cdn.robotshop.com/media/a/ard/rb-ard-33/pdf/arduinomega2560datasheet.pdf)
- [Interfacing a 2.4-inch TFT LCD display shield with Arduino](https://electropeak.com/learn/interfacing-2-4-inch-tft-lcd-display-shield-with-arduino/)
- [WT32-SC01 datasheet](https://www.electronicscomp.com/image/catalog/WT32-SC01DataSheetV3.4-with-nuts.pdf)
- [Texas Instruments LM319-N datasheet](https://www.ti.com/lit/ds/symlink/lm319-n.pdf)
- [Texas Instruments LM358 product page](https://www.ti.com/product/LM358)
- [LM7805 regulator datasheet reference](https://www.alldatasheet.com/datasheet-pdf/pdf/82833/FAIRCHILD/LM7805.html)


