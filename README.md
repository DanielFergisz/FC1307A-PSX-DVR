# FC1307A PSX DVR Firmware

Custom firmware for FC1307A SD-to-IDE adapters, adding support for the Sony PSX DVR HDD security command `0x8E`.

The original FC1307A firmware does not support this command and the PSX rejects the adapter. This version adds a proper 512-byte PIO response using the original FC1307A transfer routine.

The firmware supports SD cards larger than 128 GB and has been successfully tested with a 256 GB card.

Tested with:

- Sony PSX DESR-5000
- Sony PSX DESR-5100
- Sony PSX DESR-7000
- Sony PSX DESR-7100
- 64 GB and 256 GB SD cards
- FC1307A SD-to-IDE adapter

## Preparing the card

1. Flash the modified firmware to the FC1307A adapter.
2. Format the card using the original uLaunchELF HDD Manager on the PSX.
3. Copy the contents of the `__system` and `__sysconf` partitions.
4. Power off the console and start it again.

A full sector-by-sector HDD image is not required. The same PSX1 system files can be used on different first-generation PSX models after formatting the drive

!!! Always make a backup of the original SPI flash before programming !!!
