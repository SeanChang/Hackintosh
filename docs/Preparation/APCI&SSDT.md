## Basic Concepts

### APCI
Advanced Configuration and Power Interface (ACPI) was jointly proposed and developed by Intel, Microsoft, and Toshiba in 1997. It is an operating system power management and hardware configuration interface. ACPI defines the hardware abstraction interface between the system firmware (BIOS or UEFI) and the operating system.

It helps the operating system to reasonably control and allocate power to computer hardware devices. With ACPI, the operating system can turn off different hardware devices as needed according to the actual situation of the devices.

The main functions it covers include:

1. System power management
2. Device power management
3. Processor power management
4. Device and processor performance management
5. Configuration / Plug and Play
6. System Events
7. Battery management
8. Thermal management
9. Embedded Controller
10. SMBus Controller

In the computer application platform, ACPI is becoming more and more important. ACPI consists of many tables, including: RSDP, SDTH, RSDT, FADT, FACS, **DSDT**, **SSDT**, MADT, SBST, XSDT, ECDT, SLIT, SRAT. Among them, **DSDT** is an important description table.

### DSDT, SSDT
As mentioned above, DSDT and SSDT are part of the ACPI specification and outline hardware devices such as USB controllers, CPU threads, embedded controllers, system clocks, etc.

DSDT (Differentiated System Description Table) can be regarded as the main body containing most of the information.

SSDT (Secondary System Description Table) conveys less information.

DSDT can be regarded as a building blueprint, and SSDT is a sticky note that outlines the additional details of the project.

Under Hackintosh, DSDT is usually extracted first, and then the corresponding SSDT is written according to the content of DSDT to correct DSDT. Of course, it is also possible to directly modify the extracted DSDT without using SSDT. However, DSDT troubleshooting and code adjustment are required, and the workload is relatively large. It is not as simple and convenient as using SSDT for correction.

### Why? Why Do We Need to Know These?
macOS may be very picky about the devices existing in DSDT, so we need to correct it. The main devices that need to be corrected for macOS to work properly are:

- **EC**
  - Embedded Controller.
  - Non - Apple models all expose an EC in their DSDT, but it is usually incompatible with macOS and may cause a panic. Therefore, it needs to be hidden from macOS.
  - For laptops, the actual embedded controller still needs to be enabled for the battery and hotkeys to work. Renaming the EC will also cause problems in Windows. Therefore, it is best to create a fake EC without disabling the actual embedded controller.

- **Plugin type**
  - Plugin type.
  - Allows the use of XCPM to provide local CPU power management on Intel Haswell and newer architecture CPUs. This is not suitable for AMD.

- **AWAC system clock**
  - AWAC system clock.
  - Because macOS cannot communicate with the AWAC clock, this requires us to either force the use of the traditional RTC clock or create a fake clock for macOS to use when it is unavailable.

- **NVRAM SSDT**
  - True 300 - series motherboards (except Z370) do not declare the FW chip as MMIO in ACPI, so the kernel will ignore the MMIO region declared by the UEFI memory map. This SSDT brings back NVRAM support.

- **Backlight SSDT**
  - Used to fix backlight control support on laptops.

- **GPIO SSDT**
  - Used to allow VoodooI2C connection, only applicable to laptops.

- **XOSI SSDT**
  - Used to reroute OSI calls to this SSDT, mainly to trick our hardware into thinking that it is booting Windows so that we can get better trackpad support.

- **IRQ SSDT and ACPI patches**
  - Used to fix IRQ conflicts in DSDT, mainly for laptops. SSDT Time exclusive.
  - Note that Skylake and newer CPUs rarely have IRQ conflicts. This is mainly used on Broadwell and older systems.

## Common ACPI Combinations
ACPI can be manually compiled or downloaded from others. Although manual compilation is the most perfect, it requires a certain learning threshold. So novice users are recommended to download the compiled SSDT files.

You can download your own ACPI files according to your computer model by referring to the following content.

!!! warning "If you can't download, it's because Github is blocked in China. Here are some solutions."
    1. Use a telecom network to access and download.
    2. Use a proxy to download.
    3. Use a Github mirror site to download.
    4. Use the files in the EFI folder configured by others directly.
    5. There are always more solutions than difficulties. Don't give up when encountering a little setback.

### Intel Desktop
!!! example "Penryn, Younah, Conroe"
    - [x] [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) 

!!! example "**Lynnfield, Clarkdale**"
    - [x] [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) 

!!! example "**SandyBridge, Ivy Bridge**"
    - [ ] CPU - PM.aml
        * For power management use.
        * It needs to be generated by your own script. It's relatively old. You can refer to [Sandy and Ivy Bridge Power Management](https://dortania.github.io/OpenCore - Post - Install/universal/pm.html#sandy - and - ivy - bridge - power - management).
        * After generation, it needs to be combined with ACPI patches: **Delete CpuPm**, **Delete Cpu0Ist**.
    - [x] [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) 
    - [x] [SSDT - IMEI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - IMEI.aml) 
        * Fix the problem of mixing Ivy Bridge CPU with 6 - series motherboards.
        * Fix the problem of mixing Sandy Bridge CPU with 7 - series motherboards.

!!! example "Hasewell, Broadwell"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) 

!!! example "Skylake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml)
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 

!!! example "Kaby Lake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 

!!! example "Coffee Lake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 
    - [X] [SSDT - AWAC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - AWAC.aml) 
        * Fix the system clock on newer hardware.
        * Support the following motherboards:
            - B360, B365, H310, H370
            - Z370 (Gigabyte and AsRock motherboards with newer BIOS versions)
            - Z390
            - B460, Z490
            - 400 - series (Comet Lake) 
            - 495 - series (Ice lake)

!!! example "Comet Lake"
    - [X]  [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 
    - [X] [SSDT - AWAC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - AWAC.aml) 
        * Fix the system clock on newer hardware.
        * Support the following motherboards:
            - B360, B365, H310, H370
            - Z370 (Gigabyte and AsRock motherboards with newer BIOS versions)
            - Z390
            - B460, Z490
            - 400 - series (Comet Lake) 
            - 495 - series (Ice lake)
    - [X] [SSDT - RHUB.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - RHUB.aml) 
        * Fix the problem of some OEM motherboards. It is required to close the RHUB device and force macOS to manually rebuild the ports.
        * ASUS Z490 needs this SSDT.
        * MSI motherboards need to be tested.
        * Gigabyte and ASRock motherboards work well and don't need this SSDT.

!!! example "Rocket Lake"
    It can imitate that of the 10 - generation CPU of **Comet Lake**.
!!! example "Alder Lake"
    - [X] [SSDT - PLUG - ALT.aml](https://sqlsec.lanzoub.com/i7oos04b2m8d)
        * 12 - generation CPU big - small core scheduling, unique SSDT.
    - Others can imitate that of the 10 - generation CPU of **Comet Lake**.

### Intel Laptop
!!! note "Clarksfield, Arrandale"
    - [X] [SSDT - EC - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.

!!! note "Sany Bridge, Ivy Bridge"
    - [ ] CPU - PM.aml
        * For power management use.
        * It needs to be generated by your own script. It's relatively old. You can refer to [Sandy and Ivy Bridge Power Management](https://dortania.github.io/OpenCore - Post - Install/universal/pm.html#sandy - and - ivy - bridge - power - management).
        * After generation, it needs to be combined with ACPI patches: **Delete CpuPm**, **Delete Cpu0Ist**.
    - [X] [SSDT - EC - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.
    - [X] [SSDT - IMEI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - IMEI.aml) 
        * Fix the problem of mixing Ivy Bridge CPU with 6 - series motherboards.
        * Fix the problem of mixing Sandy Bridge CPU with 7 - series motherboards.

!!! note "Haswell, Broadwell"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
        * If this is unsuccessful, you can manually use [MaciASL](https://github.com/acidanthera/MaciASL/releases) to compile [SSDT - GPI0.dsl.zip](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/decompiled/SSDT - GPI0.dsl.zip) to replace XOSI.
            - However, the interrupt difficulty is a bit high. You can refer to the content in this part: [Trackpad Interrupt Example](/6 - Practical Poses/6 - 3/).
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.

!!! note "Skylake, Kaby Lake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
        * If this is unsuccessful, you can manually use [MaciASL](https://github.com/acidanthera/MaciASL/releases) to compile [SSDT - GPI0.dsl.zip](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/decompiled/SSDT - GPI0.dsl.zip) to replace XOSI.
            - However, the interrupt difficulty is a bit high. You can refer to the content in this part: [Trackpad Interrupt Example](/6 - Practical Poses/6 - 3/).
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.

!!! note "Coffee Lake, Whiskey Lake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
        * If this is unsuccessful, you can manually use [MaciASL](https://github.com/acidanthera/MaciASL/releases) to compile [SSDT - GPI0.dsl.zip](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/decompiled/SSDT - GPI0.dsl.zip) to replace XOSI.
            - However, the interrupt difficulty is a bit high. You can refer to the content in this part: [Trackpad Interrupt Example](/6 - Practical Poses/6 - 3/).
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.
    - [X] [SSDT - AWAC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - AWAC.aml) 
        * Fix the system clock on newer hardware.
        * Support the following motherboards:
            - B360, B365, H310, H370
            - Z370 (Gigabyte and AsRock motherboards with newer BIOS versions)
            - Z390
            - B460, Z490
            - 400 - series (Comet Lake) 
            - 495 - series (Ice lake)

!!! note "Coffee Lake Plus, Comet Lake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
        * If this is unsuccessful, you can manually use [MaciASL](https://github.com/acidanthera/MaciASL/releases) to compile [SSDT - GPI0.dsl.zip](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/decompiled/SSDT - GPI0.dsl.zip) to replace XOSI.
            - However, the interrupt difficulty is a bit high. You can refer to the content in this part: [Trackpad Interrupt Example](/6 - Practical Poses/6 - 3/).
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.
    - [X] [SSDT - AWAC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - AWAC.aml) 
        * Fix the system clock on newer hardware.
        - Support the following motherboards:
            - B360, B365, H310, H370
            - Z370 (Gigabyte and AsRock motherboards with newer BIOS versions)
            - Z390
            - B460, Z490
            - 400 - series (Comet Lake) 
            - 495 - series (Ice lake)
    - [X] [SSDT - PMC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PMC.aml) 
        * Used to support and adapt NVRAM.
        * All 300 - series motherboards need this SSDT (except Z370).
        * Support the following motherboards:
            - B360, B365
            - H310, H370 (HM370 probably doesn't need this)
            - Z390

!!! note "Ice Lake"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - LAPTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - LAPTOP.aml) 
    - [X] [SSDT - XOSI.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - XOSI.aml) 
        * Trackpad connection repair, working in polling mode by default.
        * It needs to be used with the ACPI patch: **Change _OSI to XOSI**.
        * Devices without a trackpad such as NUC don't need this.
        * If this is unsuccessful, you can manually use [MaciASL](https://github.com/acidanthera/MaciASL/releases) to compile [SSDT - GPI0.dsl.zip](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/decompiled/SSDT - GPI0.dsl.zip) to replace XOSI.
            - However, the interrupt difficulty is a bit high. You can refer to the content in this part: [Trackpad Interrupt Example](/6 - Practical Poses/6 - 3/).
    - [X] [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml)
        * Fix laptop brightness control. NUC doesn't need this.
    - [X] [SSDT - AWAC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - AWAC.aml) 
        * Fix the system clock on newer hardware.
        * Support the following motherboards:
            - B360, B365, H310, H370
            - Z370 (Gigabyte and AsRock motherboards with newer BIOS versions)
            - Z390
            - B460, Z490
            - 400 - series (Comet Lake) 
            - 495 - series (Ice lake)
    - [X] [SSDT - RHUB.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - RHUB.aml)
        * Fix the root device error on many Ice Lake laptops.

### Intel High - end Desktop
!!! abstract "Nehalem, Westmere"
    - [X] [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) 

!!! abstract "Sandy Bridge - E, Ivy Bridge - E"
    - [X] [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) 
    - [X] [SSDT - UNC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - UNC.aml)
        * Disable unused devices in ACPI to ensure that the IOPCIFamily doesn't have a kernel panic.
        * All X99 motherboards and most X79 motherboards need this SSDT.
        * In addition, some C602 and C612 motherboards also need this SSDT.

!!! abstract "Haswell - E, Broadwell - E"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 
    - [X] [SSDT - RTC0 - RANGE - HEDT.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - RTC0 - RANGE - HEDT.aml)
        * Big Sur and later systems need to ensure the compatibility of the RTC device.
    - [X] [SSDT - UNC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - UNC.aml)
        * Disable unused devices in ACPI to ensure that the IOPCIFamily doesn't have a kernel panic.
        * All X99 motherboards and most X79 motherboards need this SSDT.
        * In addition, some C602 and C612 motherboards also need this SSDT.

!!! abstract "Skylake - X/W, Cascade Lake - X/W"
    - [X] [SSDT - PLUG - DRTNIA.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PLUG - DRTNIA.aml) 
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 
    - [X] [SSDT - RTC0 - RANGE - HEDT.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - RTC0 - RANGE - HEDT.aml)
        * Big Sur and later systems need to ensure the compatibility of the RTC device.

### AMD Desktop
!!! info "Bulldozer(15h), Jaguar(16h)"
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 

!!! info "Ryzen, Threadripper(17h and 19h)"
    - [X] [SSDT - EC - USBX - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - USBX - DESKTOP.aml) 
    - [X] [SSDT - CPUR.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - CPUR.aml) 
        * Used to fix the CPU definition for B550 and A520 motherboards. Other motherboards may not need it.
        * X570 and older motherboards don't need this SSDT.

## A Complete List of ACPI for Hackintosh?

Since there are so many ACPI, it's impossible for me to list them all. Moreover, there is no authoritative explanation for the functions of some SSDT on the Internet. So the following explanations may have errors and are for reference only. If there are omissions or serious errors in the content, welcome to submit a PR on Github to supplement.

| No. | SSDT File Name | Explanation |
| ---- | ---- | ---- |
| 1 | [FixShutdown - USB - SSDT.aml](https://github.com/dortania/OpenCore - Post - Install/blob/master/extra - files/FixShutdown - USB - SSDT.dsl) | Fix the USB controller to solve the problem of automatic restart during sleep or shutdown. |
| 2 | [Spoof - SSDT.aml](https://github.com/dortania/OpenCore - Install - Guide/blob/master/extra - files/Spoof - SSDT.dsl) | Disable the GPU. |
| 3 | [SSDT - ALS0.aml](https://cn.bing.com/search?q=SSDT - ALS0.aml) | Add a virtual ambient light sensor to save the previous brightness setting after restart. |
| 4 | [SSDT - ARTC.aml](https://cn.bing.com/search?q=SSDT - ARTC.aml) | Fix the system clock found on newer hardware. It comes with OCC. |
| 5 | [SSDT - AWAC.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - AWAC.aml) | Used for 300 - series motherboards. |
| 6 | [SSDT - BAT.aml](https://cn.bing.com/search?q=SSDT - BAT.aml) | Battery patch for models like ThinkPad. |
| 7 | [SSDT - BKey.aml](https://cn.bing.com/search?q=SSDT - BKey.aml) | Used for early brightness adjustment. |
| 8 | [SSDT - BRG0.aml](https://cn.bing.com/search?q=SSDT - BRG0.aml) | May be needed if the BIOS doesn't have a Serial(COM) Port or if Super IO can't be disabled. |
| 9 | [SSDT - CPUR.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - CPUR.aml) | Energy management. For AMD B550 and A520 motherboards. X570 and older motherboards don't need it. |
| 10 | [SSDT - EC - DESKTOP.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - EC - DESKTOP.aml) | Used for old desktop platforms to repair the embedded controller. |
|
