# UserData dumps

- MCU: EFR32BG22C224F512IM40
- Read method: SWD
- Debug adapter: Raspberry Pi Pico with debugprobe firmware
- Read command: `python3 reflash.py --dump-ud`
- UserData base address: `0x0FE00000`
- Dump size: `1024 bytes`

Files:

- `userdata.txt` — console hex dump
- `userdata.bin` — raw 1024-byte UserData dump
- `userdata.bin.sha256` — SHA-256 checksum
