# UserData (UD) dumps

UD area (1024 bytes) read from an e-paper tag EL016F6W4A with EFR32xG22 while the
device was debug-locked. Made before running a debug unlock (`-u`), which
erases the flash but keeps UD.

## Device
- Tag model: EL016F6W4A
- SoC family: EFR32xG22 (SE FW version 0x1020e, debug unlock allowed)
- Exact part number: <маркировка с корпуса чипа или "unknown">

## Command
    python3 reflash.py --dump-ud-bin ud_dump1.bin \
      --pack ./SiliconLabs.GeckoPlatform_EFR32BG22_DFP.2025.12.1.pack -v

## Setup
- Adapter: Raspberry Pi Debugprobe on Pico (CMSIS-DAP), SWD
- Host: ThinkPad X220, Linux
- pyOCD 0.45.1/ Python 3.12.3
- Pack: SiliconLabs.GeckoPlatform_EFR32BG22_DFP.2025.12.1
- [reflash.py commit](https://github.com/OpenEPaperLink/Tag_FW_EFR32xG22/commit/0f09a1b696e8440a7239adc03020b94987c3fa73)

## Files
- ud_dump1.bin, ud_dump2.bin: two consecutive reads, byte-identical
- SHA256SUMS: checksums (verify with `sha256sum -c SHA256SUMS`)
- status.txt: output of `reflash.py -s`
