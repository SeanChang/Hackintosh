## Terminology

| Name             | Description                                                      |
| ---------------- | ------------------------------------------------------------ |
| **Hackintosh**   | The process of installing macOS on non - Apple - official PCs. Please note that **Hackintosh is not an operating system**. |
| **Bootloader**   | Software for loading an operating system is usually made by the creator of the operating system.                 |
| **Boot Manager** | Software for managing boot loaders such as Clover and OpenCore.             |
| **Clover**       | A boot method that is gradually being replaced by OpenCore.                       |
| **OpenCore**     | A new and more perfect Hackintosh boot method created by the [Acidanthera team](https://github.com/acidanthera). |
| **ACPI**         | Advanced Configuration and Power Interface (ACPI) enables the system to better recognize hardware information.            |
| **DSDT/SSDT**    | Tables in ACPI that describe devices and how the operating system should interact with them.          |
| **.AML**         | The compiled file format of ACPI.                                        |
| **.DSL**         | ACPI source code file.                                            |
| **Kexts**        | The full name is **K**ernel **Ext**ensions. We can commonly translate it as "driver". |
| **BIOS**         | BIOS (Basic Input/Output System), which can be generally understood as the operating system of the motherboard.            |
| **UEFI**         | Unified Extensible Firmware Interface (UEFI) enables a PC to load from a pre - boot operating environment to an operating system. |
| **UEFI 驱动**    | Like any other operating system, UEFI has drivers, which are loaded by Clover or OpenCore. |
| **NVRAM**        | Non - volatile random - access memory (NVRAM) is built - in on the motherboard. When UEFI boots, it will load NVRAM first. |

## ACPI

| Name          | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| **EC**        | Embedded Controller. It communicates between the motherboard and embedded peripheral devices (such as hotkeys, ports, or batteries). |
| **PLUG**      | Allows connection to XCPM, Apple XNU power management for better overall CPU control. |
| **AWAC**      | ACPI Wake Alarm Counter Clock, the internal clock of the board. Hackintosh must patch it. |
| **PMC**       | Power Management Controller. Some motherboards require SSDT - PMC for patching. |
| **PNLF**      | Internal backlight display. macOS uses this PNLF device to send and receive information for brightness control. |
| **XOSI/_OSI** | `_OSI` is used to determine the operating system that is booting. Renaming it to XOSI allows us to spoof the hardware. |
| **HPET**      | High - Precision Event Timer. macOS is very picky about how the device is set up, so we sometimes need to patch HPET. |
| **RHUB**      | Root USB Hub, in which USB ports are defined.                 |
| **IMEI**      | Intel Management Engine Interface, which handles miscellaneous tasks. macOS relies on IMEI for Intel GPU acceleration. |
| **UNC**       | Uncore Bridge, similar to the north bridge. It handles many cache - related functions. |
| **SMBS**      | System Management Bus, used to allow easy communication between devices. |
## UEFI Boot Disk Structure Partitioning

### UEFI - related Concepts

UEFI stands for Unified Extensible Firmware Interface. It enables a PC to load from a pre - boot operating environment to an operating system. You can think of UEFI as a system boot method. Basically, computers after 2010 support the UEFI boot method.

UEFI boot is often used in conjunction with the GPT partition table. In DiskGenius, select the disk, right - click and select [Convert partition table type to GUID format] to convert the disk to a GPT partition table.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317486851257.webp) 

The EFI partition is a necessary partition for UEFI boot. In the disk, its name is usually called the ESP partition. It is mostly located in the first partition of the disk. The format can be either FAT16 or FAT32. It is mainly used to store boot files. When installing Hackintosh, note that the ESP partition should be larger than 200MB. I, suggest setting it to 300MB, which is basically sufficient.

### Installing a Single macOS on a Single Hard Disk

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1631749573321.webp) 

It's worth mentioning that in this case, there is no need for us to manually create an EFI partition at all. When installing directly, just select the entire disk where you want to install, and macOS will automatically create disk partitions:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1631749592979.webp)

### Installing Windows and macOS on a Single Hard Disk

This is the partition situation of a 512GB SSD dual - system disk:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16322081893942.webp)

Drive C will be used as the C drive for Windows in the future. 123GB is usually sufficient for daily light - use.

Drive D will be used to install macOS at that time. 353GB is actually sufficient for daily use.

Drive E is a partition that is temporarily divided. Actually, it is the ESP boot partition. The boots of both Windows and macOS in the future will be placed here.