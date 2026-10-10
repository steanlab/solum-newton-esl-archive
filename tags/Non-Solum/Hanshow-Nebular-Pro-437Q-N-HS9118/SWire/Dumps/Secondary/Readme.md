# Original Hanshow 437Q-N firmware backup

Complete 512 KiB flash dump of an original Hanshow 437Q-N tag.

- MCU ID: `026255`
- Flash JEDEC ID: `eb6013`
- Flash capacity: 524288 bytes (512 KiB)
- Independent reads: 2
- Dumps: byte-for-byte identical
- SHA-256: see `SHA256SUMS`
- Metadata: `manifest.json`

Both binary files contain the entire flash, including application and factory data.

**Preserve these files unchanged.** They are original-device backups, not modified firmware.

## Readout equipment and method

- Programmer: Hanshow PIO Readback + UART (Zero bridge)
- Bridge serial number: `E66598541B873E39`
- Software: `nebular_debug.py`
- Software project: `Nebular_970Q_N_Programmer_Logger_Debugger_v3`
- Flash interface: SWire, `/dev/ttyACM3`
- UART interface: `/dev/ttyACM4`
- MCU ID: `026255`
- Flash JEDEC ID: `eb6013`
- Method: two independent complete 512 KiB flash reads
- Verification: both binary dumps are byte-for-byte identical and have matching SHA-256 hashes.

The UART interface was opened during the acquisition session; the flash dumps were read through the SWire interface. 
