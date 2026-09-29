# SWD access and UserData dump

## Hardware

Raspberry Pi Pico with Raspberry Pi Debug Probe firmware.

Connections:

```text
Pico GP2  -> tag SWCLK
Pico GP3  -> tag SWDIO
Pico GND  -> tag GND
```

The tag must be powered. Pico and tag must share GND.

## Software

```bash
sudo apt update
sudo apt install -y python3-pip python3-venv unzip

python3 -m venv .venv
source .venv/bin/activate
python -m pip install pyocd bincopy
```

The target is:

```text
EFR32BG22C224F512IM40
```

The required DFP is described in `packs/README.md`.

## Read UserData

Run from this directory:

```bash
python3 scripts/reflash.py --dump-ud
```

This reads 1024 bytes from:

```text
0x0FE00000–0x0FE003FF
```
