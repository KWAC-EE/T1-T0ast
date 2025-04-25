// for TUSB9261 chipset

The USB-SATA bridge requires firmware to be flashed to its SPI Flash companion in order to function at all, so don't expect it to just be plug&play

For Native Linux flashing of these chips, one possible utility can be found here: https://github.com/luxonis/tusb926x-flash-burner
TI has links to their Windows flashing utility and user guides here: https://www.ti.com/product/TUSB9261#software-development

If neither method can be used natively within the operating system, you can try using an SPI-flashing chip utility shuch as the CH341 to write the binary directly to the chip: https://www.amazon.com/Organizer-Socket-Adpter-Programmer-CH341A/dp/B07R8S8V4H?th=1
All schematics were designed to use this firmware version: https://github.com/luxonis/tusb926x-flash-burner/blob/main/flash-images/TUSB926x_FW_v1.06_SATA_NO_POLARITY_SWAP.bin
