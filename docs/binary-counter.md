# SAMD Binary Counter
**Montana Technological University — Electrical Engineering Department**  
**Author:** Miller DeMark | **Hardware Design:** Bryce Hill

---

## Overview

The SAMD Binary Counter is a student outreach tool built for Montana Tech's Electrical Engineering Department. It is designed to give prospective students a hands-on look at binary counting and embedded systems. The device counts from 0 to 1023 and displays the value in binary across a 10-LED bar, with each LED representing one bit. It also supports serial communication over USB, allowing a user to connect via terminal and interact with the counter in real time.

---

## Hardware

The PCB was designed by Bryce Hill and centers around a SAMD ARM Cortex-M0+ microcontroller. Key hardware components include:

- **SAMD ARM Microcontroller** — runs the firmware and drives the GPIO pins
- **10-LED Bar Display** — represents the 10-bit binary count across PORTA pins 0–9
- **Push Button (PA16)** — pauses and resumes counting with a single press
- **USB Port** — provides power and enables serial communication via CDC (USB-to-serial)
- **Companion Testing Board** — used during assembly to flash firmware and validate the counter before final soldering

---

## Firmware

The firmware is written in C using Atmel Start as the hardware abstraction layer. The main loop handles four responsibilities concurrently: USB communication, the debug sequence, the binary counter, and button input.

### Counting
The counter increments once per loop cycle and wraps back to 0 after reaching 1023 using a modulo operation. The speed of counting is controlled by a configurable `speed` variable (default 200ms delay per step). Counting can be paused via button press or USB command, and resumes the same way.

### LED Output
PORTA pins 0–9 are mapped directly to the LED bar. Each loop cycle, all pins are cleared and then re-driven with the current count value masked to 10 bits. This ensures the display always reflects the current count accurately.

### Button Input
A single press of the button on PA16 toggles counting on or off. The firmware uses a `buttonPressed` flag to ensure the toggle fires exactly once per physical press, regardless of how long the button is held. The flag clears on release.

### USB Serial (UART)
When a terminal connects over USB, the counter pauses and a boot message is sent to the terminal. From there, the user can issue commands to control the counter. While connected, the current count is streamed to the terminal each time it changes. The firmware handles character echo, backspace, and command parsing, making it usable in any basic serial terminal.

### Debug Mode
A debug sequence can be triggered over USB. When active, the firmware cycles through a predefined set of LED patterns one bit at a time, sweeping across all 10 LEDs. This is useful for verifying that all LEDs and their corresponding GPIO pins are functional before deployment. Normal counting resumes automatically when the sequence completes.

---

## Flashing

Firmware is flashed onto the counter using a companion Python flashing tool and the testing board. The testing board connects to the SAMD's SWD interface and allows firmware to be loaded without full final assembly. This makes it easy to flash and validate multiple units in sequence during production runs.

For detailed flashing steps, refer to the [Flashing Guide](counterflash.md).

---

## USB Commands

When connected via serial terminal, the following commands are available:

| Command | Description |
|---|---|
| `start` | Begin counting |
| `stop` | Pause counting |
| `speed <ms>` | Set counter delay in milliseconds |
| `debug` | Run the LED diagnostic sequence |
| `help` | List available commands |

---

## Project Notes

- Backwards counting was explored during development but not implemented in the current firmware version.
- The counter is designed to be self-contained and requires no additional software to operate, just power it on and it runs.
- When USB is disconnected after being connected, the counter automatically resumes and all LEDs light briefly as a reconnection indicator.
- Full source code, schematics, and additional documentation are available in the [repository](https://github.com/mdemark18/SAMD-Binary-Counter).
