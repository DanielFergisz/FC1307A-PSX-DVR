# FC1307A PSX DVR Firmware

Custom firmware for FC1307A SD-to-IDE adapters, adding support for the Sony PSX DVR vendor-specific HDD verification command `0x8E`.

The original FC1307A firmware does not support this command, causing the PSX to reject the adapter. This version adds the required 512-byte PIO response using the original FC1307A transfer routine.

The firmware supports SD cards larger than 128 GB and has been successfully tested with 256 GB and 1 TB cards.

It can also be used with compatible FC1307A-based 44-pin SD-to-IDE adapters commonly installed in PS2 Slim SCPH-700xx consoles. It extends card support beyond the original 128 GB limit, with SD cards up to 1 TB successfully tested.

## How the HDD check works

During startup, the PSX verifies the internal drive by writing `0xEC` to the ATA Features register and then issuing the vendor-specific command `0x8E`.

```text
Features = 0xEC
Command  = 0x8E

Status   = 0xD0   BSY — drive is preparing the response
Status   = 0x58   DRQ — data is ready
Data     = 256 × 16-bit words = 512 bytes
Status   = 0x50   command completed without error
```

This is not a normal sector read. The command requests a Sony-specific 512-byte identification response.

The first 128 bytes contain identification and binary data, including:

```text
Sony Computer Entertainment Inc.DESR-7000        120
```

The remaining 384 bytes are filled with zeros. If the drive does not return the expected data transfer and status sequence, the PSX rejects it.

## Video

[![Watch the FC1307A PSX DVR mod video](https://img.youtube.com/vi/S4Ubpu-f1K8/maxresdefault.jpg)](https://youtu.be/S4Ubpu-f1K8)

[Watch the full FC1307A PSX DVR modification video on YouTube](https://youtu.be/S4Ubpu-f1K8)

## Tested hardware

- Sony PSX DESR-5000
- Sony PSX DESR-5100
- Sony PSX DESR-7000
- Sony PSX DESR-7100
- 64 GB, 256 GB and 1 TB SD cards
- FC1307A SD-to-IDE adapter

## Preparing the card

1. Flash the modified firmware to the FC1307A adapter.
   - SPI flash IC: `BY25D40EST`
2. Format the card using the original uLaunchELF HDD Manager on the PSX.
   - An FMCB memory card is required to run uLaunchELF on the PSX.
3. Copy the contents of the `__system` and `__sysconf` partitions.
4. Power off the console completely and start it again.

A full sector-by-sector HDD image is not required. The same PSX1 system files can be used on different first-generation PSX models after formatting the drive.

## PSX OS v1.31

- [Polish version](https://repairbox.pl/PSX_DVR/Rev1/RepairBox_PSX1_SystemFiles_PL_v1.31.zip)
- [English version](https://repairbox.pl/PSX_DVR/Rev1/RepairBox_PSX1_SystemFiles_EN_v1.31.zip)
- [Original Japanese version](https://repairbox.pl/PSX_DVR/Rev1/RepairBox_PSX1_SystemFiles_JP_v1.31.zip)

## uLaunchELF

- [Download uLaunchELF for PSX](https://drive.google.com/file/d/1lH7ACkzD-JeFkqIqr2MXoNCG47ncfXEX/view?usp=sharing)

> [!WARNING]
> Always make a backup of the original SPI flash before programming.
