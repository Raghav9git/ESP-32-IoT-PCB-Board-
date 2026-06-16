ESP32-C3 IoT Sensor Development Board

A compact 4-layer IoT development board built around the ESP32-C3, integrating environmental sensing, local storage, USB-C power with LiPo battery management, and a programming/debug interface on a single board. Designed in KiCad 10.0.

This is a multi-domain hardware project that brings power electronics, mixed-signal sensor front-ends, digital interfaces, and RF-module integration together on one board — and routes them on a 4-layer stackup with dedicated power and ground planes.


Overview

The goal of this board is a self-contained, battery-capable IoT platform: an ESP32-C3 (Wi-Fi + BLE) as the brain, surrounded by the supporting subsystems a real IoT device needs — clean regulated power, safe battery charging, local data storage, environmental sensors, and a robust USB programming path.

It was built as a deliberate skill-development project to practice end-to-end PCB design across several engineering domains in a single, non-trivial board, rather than a series of isolated single-function boards.


Features


MCU: ESP32-C3-WROOM-02 module — Wi-Fi + Bluetooth LE
Power input: USB-C connector (power + data), with input fuse protection
Battery: LiPo battery connector with onboard charge management (MCP73871) and power-path control
Regulation: 3.3 V LDO (LM1117) for the logic rail, sourced from the 5 V USB rail
Programming/Debug: CP2102N USB-to-UART bridge with automatic boot/reset (DTR/RTS) circuit
Storage: microSD card slot (SPI) + onboard SPI flash (W25Q32)
Sensors:

BME280 — temperature, humidity, barometric pressure
TEMT6000 — ambient light
Electret microphone + MAX4466 pre-amplifier — audio/sound level



User interface: Boot and Reset buttons (with RC debounce), status LEDs, I²C OLED display header, GPIO breakout header
Debug: Test points on key SPI/SD signals
Protection: USB data-line ESD protection (USBLC6)
PCB: 4-layer design with dedicated power and ground planes



Tech Specs

ItemDetailMicrocontrollerESP32-C3-WROOM-02 (Wi-Fi + BLE)PCB layers4 (signal / GND plane / power plane / signal)Power inputUSB-C (5 V) and LiPo batteryLogic rail3.3 V (LM1117 LDO)ChargingMCP73871 LiPo charger / power pathUSB–UART bridgeCP2102NStoragemicroSD (SPI) + W25Q32 SPI flashSensorsBME280, TEMT6000, MAX4466 mic ampInterfacesI²C, SPI, UART, GPIO headerDesign toolKiCad 10.0


Subsystem Architecture

The board is organized as a set of interacting subsystems. Each block exists for a specific reason:

Power & Battery Management

USB-C delivers 5 V, passed through a fuse for input protection. The MCP73871 handles LiPo charging and automatic power-path switching between USB power and battery, so the board runs from USB when connected and from battery otherwise. The LM1117 LDO derives the clean 3.3 V logic rail from the 5 V rail. Input and output decoupling capacitors sit close to the regulator for stability.

MCU

The ESP32-C3-WROOM-02 is the core — a single-chip Wi-Fi + BLE module. It runs from the 3.3 V rail with local decoupling, and exposes the SPI, I²C, UART, and GPIO interfaces that connect to the rest of the board.

Programming & Auto-Reset

A CP2102N USB-to-UART bridge provides the serial programming/debug interface over USB-C. The DTR/RTS auto-reset circuit (two transistors) automatically drives the EN and BOOT lines during flashing, so firmware can be uploaded without manually pressing buttons — the standard ESP programming convenience circuit. USBLC6 ESD protection guards the USB data lines.

Storage

A microSD slot (SPI) provides removable local storage for data logging, and an onboard W25Q32 SPI flash offers fixed storage for configuration or firmware data. Test points on the SPI/SD lines allow probing during bring-up.

Sensors (Mixed-Signal Front-Ends)


BME280 — digital environmental sensor (temperature, humidity, pressure) on the I²C/SPI bus.
TEMT6000 — analog ambient light sensor feeding an ADC input.
Electret mic + MAX4466 — an analog audio front-end; the MAX4466 amplifies the microphone signal to a usable level for the ADC.


The analog sensor front-ends are the mixed-signal portion of the design, where layout and grounding affect signal quality.

User Interface

Boot and Reset buttons with RC debounce, status LEDs for power/operation indication, an I²C header for an OLED display, and a GPIO breakout header for expansion.


Design Highlights


4-layer stackup with dedicated ground and power planes for clean power distribution and a solid low-impedance ground return — important on a board mixing RF, analog sensors, and digital interfaces.
Mixed-signal layout — separating noisy digital and switching activity from the analog sensor front-ends.
USB & ESD considerations — controlled USB data routing and dedicated ESD protection.
Net classes for differentiated trace widths (power vs. signal).
Test points on key debug nets for hardware bring-up.
Compact, thoughtful component placement following the power and signal flow.



Project Status

~80% complete.


 Multi-page hierarchical schematic — complete, ERC passing
 Footprint assignment
 Component placement
 4-layer stackup and plane setup
 Routing (power and signal)
 Copper zones / ground pour
 Final silkscreen pass (labels, polarity marks, board info)
 Final DRC sign-off
 Manufacturing output (Gerbers / drill files)
 Fabrication and bring-up testing


Remaining work is finishing/polish: silkscreen cleanup, a final DRC pass, and generating manufacturing files.


Skills Developed

This project was built to practice the full PCB workflow on a realistic, multi-domain board:


Multi-page hierarchical schematic design
Power electronics: LDO regulation, LiPo charge management, power-path design
Mixed-signal design: analog sensor front-ends alongside digital interfaces
RF module integration: ESP32-C3 placement and supporting circuitry
Digital interfaces: SPI (flash, SD), I²C (sensors, OLED), UART (programming)
USB & ESD: USB-C, USB-to-UART bridge, data-line protection
4-layer PCB layout: stackup, dedicated power/ground planes, copper zones
Net classes, trace-width selection, and DRC-driven debugging
Reading and applying datasheets for component configuration



Tools


KiCad 10.0 — schematic capture and PCB layout
Component symbols, footprints, and 3D models sourced from KiCad libraries and standard component vendors



References & Acknowledgments

This board was built as a learning project to develop hands-on PCB design skills across power, mixed-signal, and digital domains. The design follows and adapts a published KiCad IoT development-board reference/tutorial as a starting point, which I used to study how a complete, multi-subsystem board is architected, laid out on four layers, and prepared for manufacturing.


Add the link to the specific reference design/tutorial you followed here, so the credit is explicit.



The value of the project for me was in building, routing, debugging, and understanding the full board end-to-end in KiCad — not in originating the concept. The subsystem breakdown above reflects my understanding of why each block exists and how they interact.


License

Released under the MIT License. See LICENSE for details.
