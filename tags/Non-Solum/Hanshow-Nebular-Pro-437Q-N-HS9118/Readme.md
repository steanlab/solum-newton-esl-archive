# Hanshow Nebular Pro 437Q-N (MCU: HS9118)

This directory contains hardware reverse-engineering data, PCB photos, and logic analyzer dumps for the **Hanshow Nebular Pro 437Q-N** electronic shelf label (4.37" screen).

## Hardware Overview
- **Vendor/Model:** Hanshow Nebular-Pro-437Q-N
- **MCU:** HS9118 (Highly likely a rebranded **Telink** TLSR835x/825x series, identified by the SWS pin)
- **Screen Size:** 4.37 inch 
- **Available Test Points (Pads):** `RST`, `SWS`, `TX`, `RX`, `GND`, `VCC`

## Logic Analyzer Capture (SPI Display Init)
Located in the `logic-captures` directory, you will find the `.sr` file captured via PulseView/sigrok at 5/10MHz using a Raspberry Pi Pico.

**Capture Details:**
- **Logic Channels Mapped:**
  - `CK` (SCK - Serial Clock)
  - `SDA` (MOSI - Master Out Slave In)
  - `CS` (Chip Select)
  - `DC` (Data/Command)
- **Purpose:** Extracting custom display initialization waveforms and E-Ink LUTs directly from the silent factory firmware.
