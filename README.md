# Pulse Box PCB

KiCad 9.0 hardware designs for the [PulseBox](https://github.com/michael-michelotti/pulse-box-esp) project — a modular LED grid system with a central controller driving a dynamically reconfigurable array of 8x8 LED panels. This repo holds the schematics, board layouts, and JLCPCB manufacturing outputs for all board revisions.

This is meant as a companion to the [main PulseBox repo](https://github.com/michael-michelotti/pulse-box-esp) — see that repo for the full system overview.

---

## Controller v1 (`Pulse_Box_PCB_v1/`)

<!-- TODO: Replace with labeled photo of v1 controller board -->

![Controller v1](https://github.com/user-attachments/assets/e1f9fbc6-650b-4c6b-aaa0-96a98c9dd92e)

Original controller, fabricated and assembled by JLCPCB. 2-layer board built around an **STM32F407VGT6** (ARM Cortex-M4, LQFP-100). Power comes in through a 5.5mm barrel jack and is regulated by a **TPSM53604RDA** 4A buck module (down to 5V), followed by an **AP2112K-3.3** LDO for the 3.3V MCU rail. LED data is level-shifted from 3.3V to 5V through a **SN74LV1T34** before driving the WS2812B chain. USB Micro-B for serial/flashing.

---

## Controller v2 (`Pulse_Box_PCB_v2/`)

<!-- TODO: Replace with labeled photo of v2 controller board -->

![Controller v2](https://github.com/user-attachments/assets/5c4ba8e2-3477-4321-933f-2ae8e7a9e799)

Redesigned controller centered on the **ESP32-S3-WROOM-1** module (WiFi, BLE, dual-core). 2-layer, 80mm x 80mm. The barrel jack is replaced with a 24V input to match the rest of the panel bus, and the buck module is swapped for a **discrete TPS54560B** design (24V→5V, 5A) with an **AP2112K-3.3** LDO for the 3.3V rail. USB-C exposes the ESP32-S3's native USB-JTAG for debug. Dual **SN74LV1T34** level shifters drive LED data north and west, and panel interconnect is 8-pin (24V, GND, RS-485 A/B, sense, LED data).

---

## Panel v1 (`Pulse_Box_Panel_v1/`)

<!-- TODO: Replace with labeled photo of panel board -->

![Panel v1](https://github.com/user-attachments/assets/c5d7ffe1-7d10-4821-aa7c-d2ff49c13ecc)

LED panel board, 2-layer, 80mm x 80mm. Runs an **STM32G071C8** (Cortex-M0+, LQFP-48) with four USARTs — one per edge — so adjacent panels mate face-to-face with no crossover. Power mirrors the v2 controller: 24V bus in, discrete buck to 5V for the LEDs, **AP2112K-3.3** LDO for the 3.3V MCU rail. LED data routing uses two **74HC4051** 8-channel analog muxes (one for input, one for output) so any edge can be selected as the data in/out for the panel's 8x8 grid. Four JST XH 11-position edge connectors carry 24V, GND, UART TX/RX, sense, and LED data. SWD header for debug; UART bootloader available on PA2/PA3 as a recovery path.
