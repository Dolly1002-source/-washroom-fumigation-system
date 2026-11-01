# Automated Washroom Fumigation System

An Arduino-based system that sprays sanitizing liquid in a washroom after a person leaves. A PIR motion sensor detects when the room is vacated, and a DC pump sends the liquid from a tank to a fogger nozzle. The system also checks for people before and during spraying, so it does not fumigate an occupied room.

This project was developed during an internship at **GRID R&D** (2023-2024) as part of the Department of Electronics and Communication Engineering (ECE), Dayananda Sagar Academy of Technology and Management (DSATM), Bengaluru.

---

## Problem

Public washrooms in hotels and commercial buildings can carry bacteria and viruses that spread if the space isn't sanitized regularly. Manual cleaning is inconsistent and depends on staff availability.

## Objectives

- Replace manual hygiene maintenance with an automated system
- Run a fumigation cycle automatically after each use
- Detect human presence so the room is never fumigated while occupied

## How It Works

The sketch watches the PIR sensor output (digital pin 2) and drives the pump (digital pin 8):

1. **Idle:** the pump stays off while the room is in use.
2. **Person leaves:** when the PIR output falls from HIGH to LOW, the system starts a cycle.
3. **5-second check:** it waits up to 5 seconds to see whether motion returns.
4. **Fumigation:** if the room is still empty, the pump runs for up to **10 seconds**.
5. **Safety stop:** if motion is detected at any point, the pump switches off immediately.

This assumes a PIR module whose output is HIGH while it senses motion (the common HC-SR501 behavior). If your module has the opposite polarity, invert the logic in the sketch.

## System Components

| Component | Role |
|---|---|
| Arduino Pro Mini (ATmega328P, 5 V / 16 MHz) | Microcontroller |
| FTDI cable | USB-to-serial programming |
| PIR sensor | Detects human presence |
| DC water pump | Moves liquid from the tank to the fogger |
| Single nozzle fogger | Sprays the liquid as a fine mist |
| Liquid tank | Stores the sanitizing liquid |
| LED | Indicates the fumigation process |
| Resistors, jumper wires, power supply | Supporting circuitry |
| Relay module or transistor driver (see below) | Switches the pump |

**Software:** Arduino IDE (C/C++) and Proteus Design Suite (circuit simulation).

## Wiring

| Signal | Arduino pin |
|---|---|
| PIR sensor output | D2 |
| DC pump control (and LED) | D8 |

Also connect power and ground for each component.

> **Driver note:** an Arduino pin can supply only about 40 mA, which is not enough to run a pump. Use a relay module or a transistor stage between pin 8 and the pump. The Proteus simulation in the report uses a transistor driver (TIP120).

## Getting Started

1. Open `washroom_fumigation.ino` in the Arduino IDE.
2. Select **Tools → Board → Arduino Pro or Pro Mini**, and set the processor to **ATmega328P (5V, 16 MHz)**.
3. Connect the board through the FTDI cable and select its port.
4. Upload the sketch.
5. Open the Serial Monitor at **9600 baud** to watch the PIR readings and pump status.

## Simulation

The design was simulated in Proteus in two versions:

- **Microcontroller version:** Arduino Pro Mini with PIR sensor, motor, and LED
- **Analog version:** the same behavior built with a **555 timer** and transistors, aimed at a lower-cost, smaller design

The circuit diagrams are in the project report.

## Internship Timeline

| Week | Focus |
|---|---|
| 1 | Team formation, project phases, component research, datasheet study |
| 2 | Proteus simulation, PIR-LED and PIR-motor circuits, 555 delay timer |
| 3 | Algorithm development and integration on the Arduino Pro Mini |
| 4 | Analog redesign using a 555 timer in place of the microcontroller |

## Results and Limitations

- The algorithm and both circuits were tested in Proteus simulation and refined over several iterations.
- The report does not include physical test measurements (for example spray coverage, response time, or liquid use).
- The sketch currently has a fixed 5-second wait and 10-second spray time. The report suggests optional improvements: adjustable PIR sensitivity, debouncing, configurable spray duration, and multiple pump modes.
