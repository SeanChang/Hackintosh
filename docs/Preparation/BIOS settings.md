The following BIOS settings should be referred to as much as possible. Due to the various motherboard models, it's normal if not all settings can be found. Just try to align with these settings as much as possible.

## BIOS Settings for Intel Motherboards

### Disable

| Option | Remarks |
| --- | --- |
| **Fast Boot** | Fast boot |
| **Secure Boot** | Secure boot |
| **Serial/COM Port** | Serial communication port |
| **Parallel Port** | Parallel port |
| **CSM** | `Compatibility Support Module` |
| **Thunderbolt** | Thunderbolt. It may cause some mysterious problems during installation. It is recommended to turn it off for simplicity. |
| **Intel SGX** | Also known as `Software Guard Extensions`, it is a security mechanism based on CPU hardware. |
| **Intel Platform Trust** | Intel Platform Trust Technology, mainly used for key management and security authentication services. |
| **CFG Lock** | MSR 0xE2 write - protection. It is recommended to turn it off. |
| **VT - d** | Also called `Intel® Virtualization Technology for Directed I/O (VT - d)` |

!!! note "**CFG Lock**"
    1. MSR 0xE2 write - protection.
    2. It is recommended to turn it off to improve the perfection of Hackintosh. Many laptops don't have these options, and you may need to manually unlock the hidden BIOS options.
    3. If you don't turn it off, setting the `AppleXcpmCfgLock` option to `YES` in the config.plist configuration is also okay (but the perfection is not as good as turning off CFG Lock).

!!! example "VT - d"
    1. I/O virtualization technology, which is easily confused with **VT - x**.
    2. It is recommended to turn it off. Generally, it only needs to be turned on for PVE hardware pass - through. Normal users usually don't need hardware pass - through technology.
    3. If you don't turn it off, setting the `DisableIoMapper` option to `YES` in the config.plist configuration is also okay.

### Enable

| Option | Remarks |
| --- | --- |
| **VT - x** | Also called `Intel® Virtualization Technology`, it is CPU hardware virtualization technology. |
| **Above 4G decoding** | Decoding above 4G |
| **Hyper - Threading** | Hyper - threading technology |
| **Execute Disable Bit** | A function of Intel's new - generation processors, mainly used for virus protection. |
| **DVMT Pre - Allocated: 64MB** | Memory allocated to DVMT. For laptops with 4K resolution, it is recommended to set it to 128MB or more. |
| **EHCI/XHCI Hand - off** | EHCI/XHCI hand - off |
| **OS type: Windows 8.1/10 UEFI Mode** | Operating system type |
| **SATA Mode: AHCI** | Hard disk boot mode |

## BIOS Settings for AMD Motherboards

### Disable

| Option | Remarks |
| --- | --- |
| **Fast Boot** | Fast boot |
| **Secure Boot** | Secure boot |
| **Serial/COM Port** | Serial communication port |
| **Parallel Port** | Parallel port |
| **CSM** | `Compatibility Support Module` |

### Enable

| Option | Remarks |
| --- | --- |
| **Above 4G decoding** | Decoding above 4G |
| **EHCI/XHCI Hand - off** | EHCI/XHCI hand - off |
| **OS type: Windows 8.1/10 UEFI Mode** | Operating system type |
| **SATA Mode: AHCI** | Hard disk boot mode |

!!! note "**Above 4G decoding**"
    1. It is recommended to turn it on.
    2. If you can't find it in the BIOS, adding `npci = 0x2000` to the **boot - args** boot parameter is also okay.

## Reference Cases for Some Motherboards

### ASRock Z490 Steel Legend

- 「Advanced」-「CPU Configuration」-「Intel Hyper Threading Technology」-「Enable」
- 「Advanced」-「CPU Configuration」-「CFG Lock」-「Disable」
- 「Advanced」-「CPU Configuration」-「Software Guard Extensions（SGX）」-「Disable」
- 「Advanced」-「Chipset Configuration」-「Above 4G Decoding」-「Enable」
- 「Advanced」-「Chipset Configuration」-「VT - d」-「Disable」
- 「Advanced」-「Chipset Configuration」-「Shared Memory」-「64MB」
- 「Advanced」-「Chipset Configuration」-「IGPUA Multi - Monitor」-「Enable」
- 「Advanced」-「Chipset Configuration」-「Deep Sleep」-「Enable in S4 - S5」
- 「Advanced」-「Storage Configuration」-「SATA Mode Selection」-「AHCI」
- 「Advanced」-「Intel(R) Thunderbolt」-「Discrete Thunderbolt(TM) Support」-「Disable」
- 「Advanced」-「ACPI Configuration」-「Suspend to Memory」-「Auto」
- 「Advanced」-「ACPI Configuration」-「USB Keyboard/Remote Wake - up」-「Disabled」
- 「Advanced」-「ACPI Configuration」-「USB Mouse Wake - up」-「Disabled」
- 「Advanced」-「USB Configuration」-「XHCI Hand - off」-「Enabled」
- 「Security」-「Secure Boot」-「Secure Boot」-「Disable」
- 「Security」-「Intel(R) Platform Trust Technology」-「Disable」
- 「Boot」-「Flash Boot」-「Disable」
- 「Boot」-「CSM」-「Disable」

### CoreBook X 14

- 「Advanced」-「Software Guard Extensions（SGX）」-「**Disabled**」
- 「Advanced」-「Hyper - Threading」-「**Enabled**」
- 「Advanced」-「ACPI Settings」-「Enable Hibernation」-「**Enabled**」
- 「Advanced」-「USB Configuration」-「Legacy USB Support」-「**Enabled**」
- 「Advanced」-「XHCI Hand - off」-「**Enabled**」
- 「Advanced」-「CSM Configuration」-「CSM Support」-「**Disabled**」
- 「Chipset」-「Type C Support」-「**Platform - POR**」
- 「Chipset」-「System Agent (SA) Configuration」-「VT - d」-「**Disabled**」
- 「Chipset」-「Above 4GB MMID BIOS assignment」-「**Enabled**」
- 「Chipset」-「Graphics Configuration」-「DVMT Pre - Allocated」-「**64MB**」
- 「Chipset」-「PCH - IO Configuration」-「SATA Mode Selection」-「**AHCI**」
- 「Security」-「Security Boot」-「Security Boot」-「**Disabled**」
- 「Boot」-「Quiet Boot」-「**Enabled**」
- 「Boot」-「Fast Boot」-「**Disabled**」

### GIGABYTE B360M AORUS PRO
BIOS version F3, date: 05/17/2019

- 【BIOS Features】-【Windows 8/10】-【Other Operating Systems】
- 【BIOS Features】-【CSM Support】-【Disable】
- 【BIOS Features】-【Secure Mode】-【Secure Boot Enable】-【Disable】
- 【Integrated Peripherals】-【Preset Boot Display Device】-【IGFX】(Here, the integrated graphics is used.)
- 【Integrated Peripherals】-【Intel Platform Trust Technology (PTT)】-【Disable】
- 【Integrated Peripherals】-【Software Guard Extension (SGX)】-【Disable】
- 【Integrated Peripherals】-【Trusted Computing】-【Security Device Support】-【Disable】
- 【Integrated Peripherals】-【Super IO Configuration】-【Serial Port】-【Disable】
- 【Integrated Peripherals】-【USB Programs】-【Legacy USB Support】-【Enable】
- 【Integrated Peripherals】-【Integrated Peripherals】-【XHCI Hand - off】-【Enable】
- 【Integrated Peripherals】-【Integrated Peripherals】-【USB Mass Storege Driver Support】-【Enable】
- 【Integrated Peripherals】-【Network Stack Configuration】-【Network Stack】-【Disable】
- 【Integrated Peripherals】-【SATA And RST Configuration】-【SATA Mode Selection】-【AHCI】
- 【Chipset】-【VT - d】-【Disable】
- 【Chipset】-【Internal Graphics】-【Enable】
- 【Chipset】-【DVMT Pre - Allocated】-【64M】
- 【Chipset】-【DVMT Total Gfx Mem】-【256M】
- 【Chipset】-【Above 4G Decoding】-【Enable】
- 【Chipset】-【Wake on LAN Enable】-【Disable】
- 【Power Management】-【Platform Power Management】-【Disable】
- 【Power Management】-【Mouse Wake - up】-【Double - click】

### GIGABYTE B250M - DS3H
BIOS version F9, date: 05/07/2018

- 【BIOS Features】-【Security Options】-【System】
- 【BIOS Features】-【Fast Boot】-【Disable】
- 【BIOS Features】-【Windows 8/10】-【Other Operating Systems】
- 【Integrated Peripherals】-【Preset Boot Display Device】-【PCIe Slot 1】
    - If you set IGFX integrated graphics and your monitor is plugged into the discrete graphics card, the screen will be black when you start the system.
    - If you set the PCIe slot, your integrated graphics will be disabled. When you enter the system, you will find that dual - hardware decoding is abnormal.
    - The perfect situation is to set IGFX integrated graphics, plug the monitor into the motherboard, and then plug the discrete graphics card back after entering the system.
    - Although the above operation is a bit troublesome, if you don't care about dual - hardware decoding, you can directly set the PCIe slot and plug the monitor into the discrete graphics card.
- 【Integrated Peripherals】-【SW Guard Extensions（SGX）】-【Disable】
- 【Integrated Peripherals】-【Trusted Computing】-【Security Device Support】-【Disable】
- 【Integrated Peripherals】-【Super IO Configuration】-【Serial Port】-【Disable】
- 【Integrated Peripherals】-【USB Programs】-【Legacy USB Support】-【Enable】
- 【Integrated Peripherals】-【USB Programs】-【XHCI Hand - off】-【Enable】
- 【Integrated Peripherals】-【SATA And RST Programs】-【SATA Mode Selection】-【AHCI】
- 【Chipset】-【VT - d】-【Disable】

### ASUS ROG STRIX B460 I GAMING

- 【Advanced】 - 【CPU Settings】 - 【Software Protection Extensions（SGX）】- 【Disable】
- 【Advanced】 - 【CPU Settings】 - 【Intel VMX Virtualization Technology】 - 【Enable】
- 【Advanced】- 【North Bridge】 - 【VT - d】 - 【Disable】
- 【Advanced】- 【North Bridge - 【Display Settings】 - 【Initialize IGPU】- 【Enable】
- 【Advanced】-【Trusted Computing - Security Device Support】-【Disable】
- 【Advanced】-【PCI Subsystem Settings - Above 4G Address Space Decoding】-【Enable】
- 【Boot】- 【CSM - Enable CSM】 - 【Disable】
- 【Boot】-【Boot Settings - Fast Boot】- 【Disable】
- 【Boot】-【Wait for F1 Key Press if Error Occurs】-【Disable】

### ASUS Z170I PRO GAMIMG (Hackintosh && Overclocking)

Here are some details of BIOS settings for overclocking and Hackintosh installation. Different CPUs have different characteristics. This article is for reference only.

- BIOS home page - Manual fan adjustment - All FANs **Turbo Boost**
- Advanced mode - AI Tweaker - **Ai Intelligent Overclocking** - Manual
- Advanced mode - AI Tweaker - **BCLK Frequency** - 100.0000
- Advanced mode - AI Tweaker - **ASUS Multi - Core Enhancement** - Disabled
- Advanced mode - AI Tweaker - **CPU Core Multiplier** - **Per Core** - **50 - 48 - 47 - 46** (Single - core 5.0Ghz, all - core 4.6Ghz)
- Advanced mode - AI Tweaker - Memory Speed Ratio Mode - Auto
- Advanced mode - AI Tweaker - DRAM Odd Multiplier Mode - Enabled
- Advanced mode - AI Tweaker - Memory Frequency - DDR4 - 2666Mhz
- Advanced mode - AI Tweaker - OC Adjustment - Keep Current Settings
- Advanced mode - AI Tweaker - **EPU Energy - saving Mode** - Off
- Advanced mode - AI Tweaker - **CPU SVID Support** - Disabled
- Advanced mode - AI Tweaker - DIGI + VRM - **CPU Load - line Calibration** - Level 5
- Advanced mode - AI Tweaker - DIGI + VRM - **CPU Current Capacity** - 140%
- Advanced mode - AI Tweaker - DIGI + VRM - **CPU Power Phase Number Control** - Extreme
- Advanced mode - AI Tweaker - DIGI + VRM - **CPU Power Phase Control** - Extreme
- Advanced mode - AI Tweaker - DIGI + VRM - **CPU Graphics Power Phase Control** - Extreme
- Advanced mode - AI Tweaker - DIGI + VRM - **DRAM Power Phase Control** - Extreme
- Advanced mode - AI Tweaker - Built - in CPU Power Management - Intel(R) SpeedStep(tm) - Auto
- Advanced mode - AI Tweaker - Built - in CPU Power Management - Turbo Boost Mode - On
- Advanced mode - AI Tweaker - Built - in CPU Power Management - **Long - term Power Consumption Limit** - 200
- Advanced mode - AI Tweaker - Built - in CPU Power Management - **Power Time Window** - 127
- Advanced mode - AI Tweaker - Built - in CPU Power Management - **Short - term Power Consumption Limit** - 200
- Advanced mode - AI Tweaker - **CPU Core/Cache Voltage** - Manual Mode
- Advanced mode - AI Tweaker - **CPU Core Voltage Override** - 1.280
- Advanced mode - Advanced - CPU Settings - **Hyper - Threading Technology** - On
- Advanced mode - Advanced - CPU Settings - **Enable Processor Cores** - All
- Advanced mode - Advanced - CPU Settings - **Intel Virtualization Technology** - On
- Advanced mode - Advanced - CPU Settings - **Hardware Prefetch** - On
- Advanced mode - Advanced - CPU Settings - **SW Guard Extensions(SGX)** - Off
- Advanced mode - Advanced - CPU Settings - CPU - Power Management Control - **CPU C - states** - Off
- Advanced mode - Advanced - CPU Settings - CPU - Power Management Control - **CFG Lock** - Off
- Advanced mode - Advanced - North Bridge - **VT - d** - Off
- Advanced mode - Advanced - North Bridge - **Above 4G Address Space Decoding** - On
- Advanced mode - Advanced - North Bridge - Display Settings - **Preferred Graphics Card** - Auto
- Advanced mode - Advanced - North Bridge - Display Settings - **Initialize iGPU** - On
- Advanced mode - Advanced - North Bridge - Display Settings - **DVMT Pre - Allocated** - 64M
- Advanced mode - Advanced - PCH Storage Configuration - **SATA Mode Selection** - AHCI
- Advanced mode - Advanced - PCH - FW Configuration - **TPM Device Selection** - Discreate TPM ()
- Advanced mode - Advanced - Built - in Devices - **Asmedia USB 3.1 Controller** - On
- Advanced mode - Advanced - Built - in Devices - **Asmedia USB 3.1 Fast Charging Support** - On
- Advanced mode - Advanced - Advanced Power Management (APM)- **Erp Support** - Off
- Advanced mode - Advanced - Advanced Power Management (APM)- **Power State after Power - off Recovery** - Power - off
- Advanced mode - Advanced - Advanced Power Management (APM)-  **Wake - up by PCI - E Device** - Off
- Advanced mode - Advanced - Advanced Power Management (APM)-  **Wake - up by RTC** - Off
- Advanced mode - Advanced - USB Configuration - **Legacy USB Support** - Enabled
- Advanced mode - Boot - **Fast Boot** - Disabled
- Advanced mode - Boot - Boot Settings - **POST Delay Time** - 0 seconds
- Advanced mode - Boot - Boot Settings - **Wait for F1 Key Press if Error Occurs** - Off
- Advanced mode - Boot - Boot Settings - CSM (Compatibility Support Module)- **Enable CSM** - Off
- Advanced mode - Boot - Boot Settings - Security Boot Menu - **Operating System Type** - Other Operating System