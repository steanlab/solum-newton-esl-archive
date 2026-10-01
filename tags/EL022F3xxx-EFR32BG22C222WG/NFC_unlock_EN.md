# How to Unlock NDEF Writing on an NFC Chip in Solum ESLs (Type 3 / NFC-F) Using a Phone

Some Solum M3 electronic shelf labels (ESLs) use an NFC chip with an I²C interface. A phone identifies it as NFC-F / JIS X 6319-4 / NFC Forum Type 3. Apps show “Writable: No,” even though the chip itself accepts writes over the radio interface.

## Hardware

- An Android phone with NFC.
- NFC Tools PRO: the “Advanced NFC commands” section (OTHER tab) is required. Any app capable of sending “raw” bytes over NfcF will also work.
- A Solum ESL that appears in NFC Tools as follows: Tag type: JIS 6319-4, Technologies: NfcF, Ndef, System Code: 0x12FC, Data format: NFC Forum Type 3, Writable: No.

## Procedure

### Step 1. Read the tag normally

Open the READ tab in NFC Tools and hold the tag against the phone.

*Why:* to identify what we are dealing with before sending any commands. The screen shows:

- Type 3 / NfcF, System Code 0x12FC. This is the standard system code for NDEF on Type 3. This means the format is known, and its rules are defined in the NFC Forum specification.
- Serial number. For FeliCa, this is the IDm (8 bytes), which will be needed in the commands. For example, 02:FE:42:29:0E:98:26:0C.
- Size 27 / 240 Bytes. There are 240 bytes available for NDEF (15 blocks × 16 bytes), of which 27 are currently used.
- Writable: No. This is the main problem.

### Step 2. Why “Writable: No”

In a Type 3 tag, block 0 contains a service structure (Attribute Information Block, AIB), including the RW Flag: `00` means “read-only,” while `01` means “read/write.” Android apps check this flag and use it to determine whether the tag is writable. If the flag is set to `00` (apparently by the factory firmware), it may be possible to change it.

### Step 3. Open the “raw” NFC mode in NFC Tools

Go to OTHER → Advanced NFC commands and select NfcF (JIS 6319-4) in *I/O Class*. Hold the tag against the phone. The log will show `Chip detected: <IDm>`. Normal writing through NFC Tools does not modify block 0, so direct commands are required.

### Step 4. Read block 0 (AIB)

Command (replace `<IDm>` with your own IDm from the log, without colons):

    10 06 <IDm> 01 0B00 01 8000

Enter it without spaces. Example for `02FE42290E98260C`:

    100602FE42290E98260C010B00018000

What the bytes mean: `10` is the command length, `06` is “Read Without Encryption,” followed by the IDm, `01` means one service, `0B00` is the service code `0x000B` (read), `01` means one block, and `8000` is block number 0.

The response looks like this:

    1D:07:<IDm>:00:00:01:10:02:01:00:0F:00:00:00:00:00:00:00:00:1B:00:3D

The header is `1D 07 <IDm>`, followed by status `00 00` (success), `01` (number of blocks), and then the 16 bytes of block 0:

    10 02 01 00 0F 00 00 00 00 00 00 00 00 1B 00 3D

*Why we interpret it this way:*

| Byte | Example value | Function |
|------|---------------|----------|
| 0 | 10 | Version 1.0 |
| 1 | 02 | Maximum number of blocks per read |
| 2 | 01 | Maximum number of blocks per write |
| 3-4 | 000F | 15 blocks, i.e. 240 bytes (matches NFC Tools) |
| 9 | 00 | No write operation is currently in progress |
| 10 | 00 | RW Flag: read-only (the reason for “Writable: No”) |
| 11-13 | 00 00 1B | NDEF length = 27 bytes (matches NFC Tools) |
| 14-15 | 00 3D | Checksum: sum of bytes 0-13 = `0x3D` ✓ |

The matching size and length shown by NFC Tools indicate that we are reading the structure correctly.

### Step 5. Create a backup

Take a screenshot of the READ output (or use “Save” in the menu) and record the data from the tag. You can also read blocks 1-2 containing the NDEF data:

    12 06 <IDm> 01 0B00 02 8001 8002

Writing overwrites service data, so you should be able to restore the original state if necessary.

### Step 6. Build the new block 0

1. Take the 16 bytes of block 0 from the response.
2. Change byte 10 (the eleventh byte) from `00` → `01`.
3. Increase the checksum (bytes 14-15) by 1: `003D` → `003E`, because the sum of bytes 0-13 has increased by 1.

The result is:

    10 02 01 00 0F 00 00 00 00 00 01 00 00 1B 00 3E

You cannot use someone else’s block 0 because the size (bytes 3-4) and NDEF length (bytes 11-13) differ between tags. Therefore, always start with block 0 from your own tag and recalculate the checksum.

### Step 7. Write block 0

    20 08 <IDm> 01 0900 01 8000 <16 bytes of the new block 0>

Complete example, without spaces:

    200802FE42290E98260C010900018000100201000F00000000000100001B003E

`20` is the command length, `08` is “Write Without Encryption,” `0900` is the service code `0x0009` (write), `01` means one block, and `8000` is block 0.

The tag was successfully written if the response ends with status `00:00`, for example:

    0C:09:<IDm>:00:00

Any other value means that the write was not accepted by the chip.

### Step 8. Verify

- Repeat the block 0 read (Step 4): byte 10 should now be `01`, and the checksum should be `003E`.
- In the **READ** tab, it should now show **“Writable: Yes”**.

### Step 9. Write your own data

Go to **WRITE → Add a record** (Text or URL) → **Write**, and keep the tag near the phone until confirmation. Verify the result in the READ tab.

## Rollback

Write block 0 again with RW Flag `00` and the original checksum (in this example: `… 00 00 1B 00 3D`).
