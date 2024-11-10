## Basic Concepts

The full English name of Kext is **K**ernel **Ext**ension, that is, kernel extension. We can simply understand it as the driver of macOS. The usage method is just to put these kext files into the `EFI/OC/kexts` folder, and then edit the OC configuration file to load these kexts and adjust the order properly.

## Download Kexts

Here, I will briefly list some methods for downloading Kexts:

1. Use third - party OC editor software such as OpenCore Configurator to download.
2. Use a search engine to search for the address under the Github of Kexts, and manually look for the compiled kext files in Releases.
3. Use the official OC download page to download common kexts: [https://dortania.github.io/builds/](https://dortania.github.io/builds/)

!!! warning "If you can't download, it's because Github is blocked in China. Here are several solutions."
    1. Use a telecom network to access and download.
    2. Use a proxy to download.
    3. Use a Github mirror site to download.
    4. Use the files in the EFI folder that others have already configured and use them directly.
    5. There are always more solutions than difficulties. Don't give up just because of a little setback.

## Classification of Kexts

### Essential Drivers

If the essential kexts are missing, your Hackintosh system will not be able to start.

- [X] [**VirtualSMC.kext**](https://github.com/acidanthera/VirtualSMC/releases)
    * Simulate the SMC chip of a genuine Mac.
    * Replace the old FakeSMC.
    * Only support OS X 10.6 + versions of the system.
- [X] [**Lilu.kext**](https://github.com/acidanthera/Lilu/releases)
    * It is a dependency of many famous kexts. Without Lilu, AppleALC, WhateverGreen, VirtualSMC, etc. cannot be used normally.
    * Only support OS X 10.8 + versions of the system.

### VirualSMC Plugins

When you download the compiled kexts of VirtualSMC, you will find that there are other kexts in it. These other kexts are plugins of VirtualSMC. Here are the functions of these plugins:

- [ ] **SMCBatteryManager.kext**
    * For laptops only, used to correctly read and display battery capacity.
- [ ] **SMCDellSensors.kext**
    * For some Dell machines only. Generally, machines that are not Dell don't need to use it.
    * For Dell machines that support SMM (System Management Mode), it can monitor and control the fan more accurately.
- [ ] **SMCLightSensor.kext**
    * For laptops only, used for the ambient light sensor on laptops.
    * Most laptops don't have this sensor, so even if it's used, it's only pseudo - light - sensing.
- [ ] **SMCProcessor.kext**
    * Used to monitor the CPU temperature, applicable to both desktops and laptops.
    * Doesn't support AMD CPUs.
- [ ] **SMCSuperIO.kext**
    * Used to monitor the fan speed, applicable to both desktops and laptops.
    * Doesn't support AMD CPUs.

### Graphics Card Drivers

- [X] [**WhateverGreen.kext**](https://github.com/acidanthera/WhateverGreen/releases)
    * Basically, all integrated graphics and discrete graphics need to use this kext.
    * Used for graphics patching, DRM repair, buffer repair, etc.
    * Only support OS X 10.8 + versions of the system.

### AMD Graphics Card Sensors

- [X] [**RadeonSensor.kext and SMCRadeonGPU.kext**](https://github.com/aluveitie/RadeonSensor/releases)
    * Since Radeon VII, Apple has stopped directly reporting the temperature, and kexts are required to intervene and implement this function.
    * For Vega 10 and earlier versions, other tools can already display the GPU temperature without additional kexts.
    * Support all GPUs from Radeon HD 7000 series to RX 6000 series.
    - [ ] **RadeonSensor.kext**
        * Required to read the GPU temperature, and Lilu is needed.
    - [ ] **SMCRadeonGPU.kext**
        * Can be optionally used to export the GPU temperature to VirtualSMC for monitoring tools to read.
    - [ ] **RadeonGadget.app**
        * Displays the GPU temperature in the status bar, and only needs to load `RadeonSensor.kext`.

### Sound Card Drivers

- [X] [**AppleALC.kext**](https://github.com/acidanthera/AppleALC/releases)
    * Used for AppleHDA patching, supports most on - board sound card drivers.
    * The `AppleALCU.kext` in the folder is a streamlined version of AppleALC, only supporting digital audio.
    * AMD motherboards and CPUs may encounter some problems, and it's rare to be able to drive the microphone.
    * Only support OS X 10.8 + versions of the system.
    * For a detailed list of sound cards that support native drivers, refer to: [Hackintosh Sound Card Driver Status Table and Layouts id Status](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    * After injecting this driver, just add alcid = xxxx in the boot - args.
- [X] [**VoodooHDA.kext**](https://sourceforge.net/projects/voodoohda/)
    * A relatively old and classic sound card driver, also known as the universal sound card driver.
    * If AppeALC.kext can't drive, you can consider this one.
    * But the usage experience is definitely not as perfect as the native AppleALC.kext.
    * Only support OS X 10.6 + versions of the system.

### USB Drivers

In later versions of macOS 11, the traditional USB driver customization method is no longer effective. Currently, the most perfect method is to use USBToolBox in Windows to customize USB, and finally use Hackintool for simple fine - tuning and correction.

- [X] [**USBToolBox.kext and UTBMap.kext**](/High Customization/USB customization/)
    - [ ] **USBToolBox.kext **
        * The official download address is: https://github.com/USBToolBox/kext/releases.
    - [ ] **UTBMap.kext**
        * It needs to be generated by referring to the [USB Customization Tutorial](/High Customization/USB customization/).
    * It is recommended to customize USB before installing the system to avoid unnecessary troubles later.
- [X] [**XHCI - unsupported.kext**](https://sqlsec.lanzoub.com/i8CUI046dufe)
    * Used when the USB 3.X interface still doesn't work properly after USB customization is completed.
    * Commonly used for motherboards of 400 - series and above.
    * If the motherboard is not natively supported by macOS USB drivers, this is needed.

!!! warning "**The following are old tutorials. They are not deleted and are kept for commemoration and reference.**"

> - [X] [**USBInjectAll.kext**](https://bitbucket.org/RehabMan/os - x - usb - inject - all/downloads/)
>     * RehabMan's previous USB driver.
>     * Version 0.7.1 released in November 2018 is the last version, and there has been no update since then.
>     * Used to inject Intel USB controllers on systems where USB ports are not defined in ACPI.
>     * Desktop CPUs of Skylake + don't need this.
>     * For AsRock motherboards, this may still be needed.
>     * Coffee Lake seems to still need this.
>     * CPUs before Skylake theoretically also need this.
>     * Support OS X 10.11 + versions of the system.
> 
>     When writing this, I have mixed feelings. RehabMan can be regarded as a veteran in the Hackintosh community. He is also a moderator of Tonymacx86. Many famous Hackintosh kexts are from him. However, for various reasons, he has not been active since 2018, disappearing as if he had never been here. But there are still legends about him in the community: [Can we all thank RehabMan](https://www.reddit.com/r/hackintosh/comments/hbu5an/can_we_all_thank_rehabman/).
> 
>     I really admire such a person. He has been answering questions in the forum for ten years, regularly updating these open - source kexts. Even some Apple developers come to learn from him. The irony of Hackintosh is that there are too many people who are free - riders and have no sense of gratitude. Maybe when they encounter difficulties in installing the system, they will go to your Github to submit an issue and urge you to update, as if you are responsible for them just because you open - source this driver. After a successful installation, the questioners disappear, not even saying thank you, and not leaving any valuable document information. This will cause many experts to answer the same repetitive and low - tech questions every day. If it were me, I definitely couldn't last for a few days. But RehabMan has persisted for more than 10 years. It's really shocking. RehabMan's last post on TonymacX86 said: "**I'm still here, but busy with other (real - life) things. I won't be able to answer questions here. People need to learn to read.**" Hopefully, it's really the case, rather than being deeply hurt by these ungrateful people.
> 
> - [X] [**USBInjectAll.kext**](https://github.com/daliansky/OS - X - USB - Inject - All/releases)
>     * The version maintained by the domestic Hackintosh Xiaobing.
>     * An updated and improved version based on RehabMan's.
>     * Supports new 400 - and 500 - series motherboards.
> 
>     Speaking of Hackintosh Xiaobing, I also admire such a person. When I first got to know him, I thought he was a middle - aged man in his 30s. But later, I learned that his son has already gone to college, and he is almost at retirement age.
> 
>     Like RehabMan, Hackintosh Xiaobing has also written many Hackintosh tutorial articles, open - sourced drivers for many models, and has always provided Hackintosh lazy - person mirror download packages for many years. The most important thing is that at almost retirement age, he still insists on doing the relatively "fashionable" Hackintosh technology. It's really amazing. I don't know if I can still do this when I'm old. Ha ha~ If Hackintosh technology still exists.

### Wired Network Card Drivers

- [X] [**AtherosE2200Ethernet.kext**](https://github.com/Mieze/AtherosE2200Ethernet/releases)
    * Required for Atheros Qualcomm and Killer network cards.
    * Note: The Atheros Killer E2500 model is actually based on Realtek, so please use the RealtekRTL8111 driver.
    * Support OS X 10.8 + versions of the system.
- [X] [**IntelMausi.kext**](https://github.com/acidanthera/IntelMausi/releases)
    * Drivers for most Intel network cards.
    * Network cards based on the I211 chipset need to use the SmallTreeIntel82576 kext.
    * Officially supports Intel's 82578, 82579, I217, I218, and I219 network cards.
    * For a detailed list of wired network card models supported by the driver, refer to: https://github.com/acidanthera/IntelMausi.
    * Requires OS X 10.9 or a later version. Old users of 10.6 - 10.8 can use IntelSnowMausi instead.
- [X] [**LucyRTL8125Ethernet.kext**](https://www.insanelymac.com/forum/files/file/1004 - lucyrtl8125ethernet/) 
    * Driver for Realtek's 2.5Gb network card.
    * The official page requires registration to download. You can also download the version I uploaded [Lanzou Cloud](https://sqlsec.lanzouw.com/iO7rru6awad).
    * Requires macOS 10.15 + versions of the system.
- [X] [**RealtekRTL8111.kext**](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
    * Drivers for most Realtek gigabit network cards.
    * Note: Sometimes the latest version of the kext may not work properly. In this case, you can try using an older version.
- [X]  [**SmallTreeIntel82576.kext**](https://github.com/khronokernel/SmallTree - I211 - AT - patch/releases)
    * I211 wired network card driver.
    * Most AMD motherboards with Intel wired network cards need this.
    * Version support: OS X 10.9 - 12 (v1.0.6), macOS 10.13 - 14 (v1.2.5), macOS 10.15 + (v1.3.0).

!!! note "Other wired network cards that don't require kexts"
    - [ ] **Intel I225 - V**
        * Some high - end Comet Lake motherboards will be equipped with this I225 - V 2.5GBe wired network card.
        * This network card is a bit troublesome. The driving method changes in different versions.
        * If the network card is not driven properly, **the system will crash, black screen, and restart a few minutes after entering the system**.
        * For the driving method, refer to [Common QA](/FAQ/Usage method/).
    - [ ] **Intel I350**
        * Add `PciRoot(0x0)/Pci(0x1,0x1)/Pci(0x0,0x0)` in the device properties of the OC configuration file. For example,
            - device - id `33150000` of type DATA.
            - The path is different for different models. The actual path should be based on the one viewed by Hackintool.
        * Requires OS X 10.10 or a later version.

!!! note "Some relatively old 100 - megabit wired network card drivers"
    - [X] [**AppleIntelE1000e.kext**](https://github.com/chris1111/AppleIntelE1000e/releases)
        * Mainly related to Intel wired network cards based on 10/100MBe.
        * Requires 10.6 or a higher version.
    - [X] [**RealtekRTL8100.kext**](https://www.insanelymac.com/forum/files/file/259 - realtekrtl8100 - binary/) 
        * Supported network card models include RTL8101E, RTL8102E, RTL8103E, RTL8401E, RTL8105E, RTL8402, RTL8106E, RTL8106EUS, RTL8107E.
        * The official page requires registration to download. You can also download the version I uploaded [Lanzou Cloud](https://sqlsec.lanzouw.com/itn5Eu6hykh).
    - [X] [**BCM5722D.kext**](https://github.com/chris1111/BCM5722D/releases) 
        * Broadcom's wired network card driver.
        * Supported network card models include BCM5722, BCM5754, BCM5754M, BCM5755, BCM5755M, BCM57788, BCM5787, BCM5787M, BCM5906, BCM5906M.
        * Requires OS X 10.6 or a later version.

### Wireless Network Card Drivers

#### Intel Wireless Network Card Series

The drivers transplanted by the great [zxystd](https://github.com/zxystd) in China from Linux OpenBSD are very hardcore and highly completed. They can be used normally for relay. AirDrop can only be recognized currently, and file transfer is not available for the moment, but it's already great.

- [X] [**AirportItlwm.kext**](https://github.com/OpenIntelWireless/itlwm/releases)
    * WiFi driver for Intel network cards.
    * List of supported Intel wireless network card models: https://docs.oiw.workers.dev/itlwm/Compat.html.
    * Only support macOS 10.13 and higher versions.
- [X] [**IntelBluetoothFirmware.kext and IntelBluetoothFirmware.kext**](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)
    * Bluetooth drivers for Intel network cards, used with `AirportItlwm.kext`.
    * Only support macOS 10.13 and higher versions.
    * If you are sure that your network card model is supported but the bluetooth doesn't work, then most likely your USB has not been customized properly.
    * After macOS 12, there are small changes in the bluetooth driving method. For details, refer to: [macOS 12 Bluetooth](/High Customization/Bluetooth in macOS 12/).

#### Broadcom Driver - free Series

There are many driver - free network card models. You can refer to [OC official wireless network card purchase guide](https://dortania.github.io/Wireless - Buyers - Guide/).

- [X] [**AirportBrcmFixup.kext**](https://github.com/acidanthera/AirportBrcmFixup/releases)
    * Wireless network card driver for non - original Apple wireless network cards or non - Fenvi Broadcom network cards.
    * Support OS X 10.10 and higher versions.
- [X] [**BrcmPatchRAM series**](https://github.com/acidanthera/BrcmPatchRAM/releases)
    * Bluetooth drivers for all non - Apple/non - Fenvi wireless network cards.
    - [ ] BrcmPatchRAM.kext   : For systems of 10.8 - 10.10.
    - [ ] BrcmPatchRAM2.kext  : For systems of 10.11 - 10.14.
    - [ ] BrcmPatchRAM3.kext  : For systems of 10.15 +.

!!! warning "Several details of Broadcom network cards. For Big Sur and later systems, due to some abnormalities in the driver, *AirPortBrcm4360_Injector.kext* needs to be manually deleted."

Bluetooth loading requires a certain order. The following is the bluetooth loading order for systems of 10.15 + in `Kernel -> Add`:
1. BrcmBluetoothInjector.kext
2. BrcmFirmwareData.kext
3. BrcmPatchRAM3.kext

After macOS 12, there are small changes in the bluetooth driving method. For details, refer to: [macOS 12 Bluetooth](/High Customization/Bluetooth in macOS 12/).


### Other Drivers

- [X] [**VoodooPS2Controller.kext**](https://github.com/acidanthera/VoodooPS2/releases)
    * If the keyboard or mouse on the installation interface of your desktop computer doesn't work, remember to use this kext.
    * It is suitable for systems equipped with PS2 keyboards, mice and touchpads.
    * The MT2 (Magic Trackpad 2) function requires macOS 10.11 or a later version.

- [X] [**CpuTscSync.kext**](https://github.com/acidanthera/CpuTscSync/releases)
    * TSC synchronization is required on some Intel HEDT and server motherboards. Without this, macOS may be very slow or even unable to start.
    * This kext may also be required on some relatively new laptops.
    * It is not applicable to AMD CPUs.
    * Requires OS X 10.8 or a later version.
    * macOS 12 compatibility has been added for CPUs with `MSR_IA32_TSC_ADJUST`(03Bh).
- [X] [**NVMeFix.kext**](https://github.com/acidanthera/NVMeFix/releases)
    * Used to repair power management and initialization on non - Apple NVMe.
    * It is recommended to use this kext for laptops, which helps to reduce power consumption during sleep.
    * It's okay for desktops not to use this, after all, few people care about the power consumption of desktops during sleep.
    * Requires macOS 10.14 or a later version.
- [X] [**HibernationFixup.kext**](https://github.com/acidanthera/HibernationFixup/releases)
    * A Lilu plugin designed to fix hibernation compatibility issues.
    * Solve the problems that the Hackintosh system can't wake up, freezes, or has a black screen after sleep.
- [X] [**SATA - unsupported.kext**](https://raw.githubusercontent.com/khronokernel/Legacy - Kexts/master/Injectors/Zip/SATA - unsupported.kext.zip)
    * For laptops, if the SATA hard disk drive can't be seen in macOS, you can consider using this.
- [X] [**CtlnaAHCIPort.kext**](https://raw.githubusercontent.com/dortania/OpenCore - Install - Guide/master/extra - files/CtlnaAHCIPort.kext.zip)
    * For laptops, when the SATA hard disk drive can't be seen in macOS under macOS 11.X +, use this.

### Commonly - used AMD Drivers

- [X] [**AMDRyzenCPUPowerManagement.kext**](https://github.com/trulyspinach/SMCAMDProcessor/releases)
    * Power management driver for AMD processors.
- [X] [**SMCAMDProcessor.kext**](https://github.com/trulyspinach/SMCAMDProcessor/releases)
    * Sensor monitoring and VirtualSMC plugin for AMD processors.
- [X] [**AppleMCEReporterDisabler.kext**](https://raw.githubusercontent.com/AMD - OSX/AMD_Vanilla/master/Extra/AppleMCEReporterDisabler.kext.zip)
    * Used to turn off AppleMCERReport.
    * AppleMCERReport will cause kernel crashes for AMD CPUs.
    * It may also be helpful for some motherboards with dual CPUs.
    * The affected SMBIOS are: MacPro6,1, MacPro7,1, iMacPro1,1.
    * Requires macOS 10.15 or a later version.

- [X] [**XLNCUSBFix.kext**](https://cdn.discordapp.com/attachments/566705665616117760/566728101292408877/XLNCUSBFix.kext.zip) 
    * USB repair for AMD FX systems, not recommended for Ryzen.
    * Requires macOS 10.13 or a later version.
- [X] [**VoodooHDA.kext**](https://sourceforge.net/projects/voodoohda/)
    * Supports audio for FX systems and front - panel microphone and speaker for Ryzen systems.
    * Do not mix with AppleALC.
    * It is a relatively old and classic sound card driver, also known as the universal sound card driver.
    * If AppeALC.kext can't drive, you can consider this one.
    * But the usage experience is definitely not as perfect as the native AppleALC.kext.
    * Only support OS X 10.6 + versions of the system.

### Commonly - used Drivers for Laptops

#### Input Device Drivers

- [X] [**VoodooPS2Controller.kext**](https://github.com/acidanthera/VoodooPS2/releases)
    * It is suitable for systems equipped with PS2 keyboards, mice and touchpads.
    * The MT2 (Magic Trackpad 2) function requires macOS 10.11 or a later version.
- [X] [**RehabMan's VoodooPS2Controller.kext**](https://bitbucket.org/RehabMan/os - x - voodoo - ps2 - controller/downloads/)
    * It has better support for older systems. For new systems, it is recommended to use the above kext.
    * For old systems with PS2 keyboards, mice and touchpads, or when you don't want to use VoodooInput.
    * Support macOS 10.6 +.
- [X] [**VoodooRMI.kext and VoodooSMBus.kext**](https://github.com/VoodooSMBus/VoodooRMI/releases)
    * Touchpad drivers for systems with Synaptics SMBus devices.
    * Mainly used for touchpads and trackpoints. The ThinkPad red dot can also be driven.
    * The MT2 (Magic Trackpad 2) function requires macOS 10.11 or a later version.
- [X] [**VoodooSMBus.kext**](https://github.com/VoodooSMBus/VoodooSMBus/releases)
    * Touchpad drivers for systems with ELAN SMBus - based devices.
    * Mainly used for touchpads and trackpoints.
    * Currently support macOS 10.14 or a later version.
- [X] [**VoodooI2C.kext**](https://github.com/VoodooI2C/VoodooI2C/releases)
    * Touchpad drivers for repairing I2C devices.
    * Generally, they are some more advanced touchpads or touchscreens.
    * The MT2 (Magic Trackpad 2) function requires macOS 10.11 or a later version.
    * This is often used with PS2 kexts and requires fine - tuning, otherwise errors may occur. For details, see: [Kexts Fine - tuning](/Preparation/Kexts fine - tuning/).
    * Some plugins of VoodooI2C:
        - [ ] VoodooI2CHID.kext
            * Microsoft HID driver, also supports some models of touchscreens.
        - [ ] VoodooI2CELAN.kext
            * For ELAN only. For ELAN1200 + versions, VoodooI2CHID.kext is required instead.
        - [ ] VoodooI2CSynaptics.kext
            * For Synaptics only. For Synaptics F12 protocol, VoodooI2CHID is required instead.
        - [ ] VoodooI2CFTE.kext
            * FTE1001 touchpad.
        - [ ] VoodooI2CAtmelMXT.kext
            * Atmel multi - touch protocol.
        - Before using, first determine the model of your touchpad. Generally, only one corresponding kext is required for a laptop.

#### Other Drivers

- [X] [**ECEnabler.kext**](https://github.com/1Revenger1/ECEnabler/releases)
    * Fixes the problem of reading battery status on most laptops (allows reading of EC fields longer than 8 bits).
- [X] [**BrightnessKeys.kext**](https://github.com/acidanthera/BrightnessKeys/releases)
    * Driver for laptop brightness shortcut keys.
- [X] [**AsusSMC.kext**](https://github.com/hieplpvip/AsusSMC/releases)
    * VirtualSMC plugin dedicated to Asus laptops.
    * Provides ALS, keyboard backlight and Fn key drivers, and supports battery monitoring and charging.
    * Supports Asus laptops equipped with ATK devices.
- [X] [**CPUFriend.kext and CPUFriendDataProvider.kext**](https://github.com/acidanthera/CPUFriend/releases)
    * Can adjust the turbo - boost performance of the macOS CPU frequency.
    * Need to cooperate with scripts to generate kexts for your own models. Refer to the [official tutorial](https://github.com/acidanthera/CPUFriend/blob/master/Instructions.md).
- [X] [**RealtekCardReaderFriend.kext and RealtekCardReader.kext**](https://github.com/0xFireWolf)
    * Realtek card reader drivers for macOS.
    * [RealtekCardReaderFriend.kext](https://github.com/0xFireWolf/RealtekCardReaderFriend/releases)
    * [RealtekCardReader.kext](https://github.com/0xFireWolf/RealtekCardReader/releases)
    * Just use these two kexts together, and they require Lilu 1.4.7 + version as a dependency.


## A Complete List of Hackintosh Kexts?

Since there are too many and too complicated Kexts, the workload is too large. I will directly post some addresses of resources, and you can check them by yourselves:

- [Commonly - used Kexts of OpenCore](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Kexts.md)
- [Some relatively old Kexts](https://docs.google.com/spreadsheets/d/15S - ocrkm_VTUJpKxNII - YuQFd5VYdjbe0DHlZVCQyM)
- [Some Kexts based on Lilu](https://github.com/acidanthera/Lilu/blob/master/KnownPlugins.md) 

If you are a patient person, if you sort out a complete list of Kexts, welcome to submit a PR.