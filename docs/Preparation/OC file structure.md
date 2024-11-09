## Original File Structure

The official project address of OpenCore boot is: [https://github.com/acidanthera/OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)

The download address for viewing the latest version is: [https://github.com/acidanthera/OpenCorePkg/releases](https://github.com/acidanthera/OpenCorePkg/releases)

As of September 17th, 2021, when this article was written, the latest OC boot version was 0.7.3. So we downloaded OpenCore - 0.7.3 - RELEASE.zip and unzipped it, and then we got the original files of OC.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318786655239.webp) 

The functions of these directories are briefly described as follows:

- **Docs**: Stores the latest OC configuration documents, version update changes, ACPI sample files, and the Sample.list configuration file template.
- **IA32**: Contains the EFI boot files used for 32 - bit old - fashioned machines.
- **Utilities**: The small tools integrated by OC official are placed here.
- **X64**: Contains the EFI boot files used for current mainstream 64 - bit machines.

Now that we know the general directory structure, let's introduce the details of the files in these directories.

## Docs

```bash
~/Downloads/OpenCore - 0.7.3 - RELEASE/Docs
.
├── AcpiSamples          # ACPI sample files
│   ├── Binaries         # Compiled ACPI files
│   │   ├── SSDT - ALS0.aml
│   │   ├── SSDT - AWAC - DISABLE.aml
│   │  ...
│   └── Source           # Original ACPI files
│       ├── SSDT - ALS0.dsl
│       ├── SSDT - AWAC - DISABLE.dsl
│      ...
├── Changelog.md         # Version update log
├── Configuration.pdf    # The official configuration document of the current version
├── Differences.pdf      # The changed parts compared with the previous version
├── Sample.plist         # The configuration file template of the current version
└── SampleCustom.plist   # The configuration file template of the current version
```

The files here are quite easy to understand. Besides, there seem to be two configuration file templates, namely Sample.plist and SampleCustom.plist. But I compared them and found that there was basically no difference between them. Theoretically, both of them can be used, and we generally use the `Sample.plist` configuration file more often.

## Utilities

This directory contains some small tools of OC official. The functions of these small tools are introduced as follows:

- **acdtinfo**: Detects the installation status of kexts on the current machine.
- **ACPIe**: Generates useful ACPI lookup and tracking for troubleshooting.
- **CreateVault**: Contains an RSA key - generation tool and a script for creating a Valut.
- **disklabel**: A small label - generation tool, which is generally not needed.
- **icnspack**: A small icns synthesis and production tool, which can be used when you want to customize the theme by yourself.
- **kpdescribe**: Used for debugging and troubleshooting, and for restoring stack traces.
- **LegacyBoot**: Tools and scripts for simulating the UEFI environment on old computers.
- **LogoutHook**: An enhanced script used to simulate NVRAM saving.
- **macrecovery**: A script for creating a boot disk.
- **macserial**: A serial - number - generation tool.
- **ocpasswordgen**: An OpenCore password - data - generation tool.
- **ocvalidate**: Checks whether the syntax of config.list is correct or not.

## EFI

Finally, we come to the important part, the EFI part. The functions of the main EFI folders are explained as follows:

```bash
~/Downloads/OpenCore - 0.7.3 - RELEASE/X64/EFI
.
├── BOOT                          # Boot folder
│   └── BOOTx64.efi               # Boot file
└── OC                            # OC folder
    ├── ACPI			          # ACPI storage folder
    │   ├── SSDT - EC.aml
    │   ├── SSDT - PLUG.aml
    │   ├── SSDT - PNLF.aml
    │      ...
    ├── Drivers                   # OC drivers folder
    │   ├── AudioDxe.efi
    │   ├── CrScreenshotDxe.efi
    │   ├── HiiDatabase.efi
    │      ...
    ├── Kexts                     # Folder for storing kernel extension kexts
    │   ├── AppleALC.kext
    │   ├── Lilu.kext
    │   ├── WhateverGreen.kext
    │      ...
    ├── OpenCore.efi              # The core file of OC
    ├── Resources                 # The theme style of OC
    │   ├── Audio
    │   ├── Font
    │   ├── Image
    │   └── Label
    └── Tools                     # OC small - tools folder
        ├── BootKicker.efi
        ├── ChipTune.efi
        ├── CleanNvram.efi
           ...
```

### BOOT

The BOOT boot folder contains the BOOTx64.ef boot file.

### OC/ACPI

The compiled SSDT files are stored here, and the format is all.aml. For detailed information about SSDT, you can refer to [the following section](/Preparatory work/APCI&SSDT/)

### OC/Drivers

Some driver files are placed here, and these drivers all end with the.efi suffix. The OC official default driver files and their descriptions are as follows:

- **AudioDxe.efi**: Used to play the "Duang" sound during startup, just like a genuine Mac.
- **CrScreenshotDxe.efi**: The screenshot - driving of the OC boot interface. Pressing F10 will save a screenshot of the current interface to the root directory of the EFI partition.
- **HiiDatabase.efi**: Used to support UEFI font rendering, and it is generally not needed after the fourth - generation Core.
- **NvmExpressDxe.efi**: Used to enable old motherboards to support NVME Express devices, and it is generally not needed after the fourth - generation Core.
- **OpenCanopy.efi**: A necessary driver for using a graphical OC theme.
- **OpenHfsPlus.efi**: A file - system driver, used to support and recognize the HFS + disk format.
- **OpenLinuxBoot.efi**: A newly - added driver in OC 0.7.3, used to boot the Linux system.
- **OpenPartitionDxe.efi**: A partition - management - driver program. Used to load the DMG image of the old - version macOS.
- **OpenRuntime.efi**: A necessary and powerful core driver of OC. Just remember that this is a necessary driver.
- **OpenUsbKbDxe.efi**: A USB keyboard driver, used to simulate Apple hotkeys, and it is an equivalent solution to KeySupport.
- **Ps2KeyboardDxe.efi**: A PS/2 keyboard driver. The PS/2 keyboard is too old. I haven't seen it for many years.
- **Ps2MouseDxe.efi**: A PS/2 mouse driver. It's also too old, and I haven't seen it for many years.
- **UsbMouseDxe.efi**: A USB mouse driver. Some virtual machines need to rely on this driver to use the mouse on the boot interface.
- **XhciDxe.efi**: The XHCI USB controller driver. Basically, most firmwares have had this driver since the second - generation Core.

### OC/Kexts

Some Kexts kernel - extension files are placed here, and the format is all.kext. For detailed information about Kexts, you can refer to [the following section](/Preparatory work/APCI&SSDT/)

### Resources

The third - party theme files of OC are placed here. Actually, I have always been using the [official theme](https://github.com/acidanthera/OcBinaryData), which is quite simple.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318842917381.webp) 

### Tools

The OC small - tools folder. Tools like CleanNvram.efi and ResetSystem.efi are some of them:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1631884661477.webp)   

The official OC - built - in tool files and their descriptions are as follows:

-  **BootKicker.efi**: Calls the original Apple - native boot - switching GUI and is for genuine Macs. Hackintosh does not support it.
-  **ChipTune.efi**: Tests the BeepGen protocol and generates audio signals of different styles and lengths.
-  **CleanNvram.efi**: An NVRAM - cleaning tool. Actually, the NVRAM - cleaning function that OC comes with is already sufficient.
-  **ControlMsrE2.efi**: Checks the consistency of CFG locking (MSR 0xE2 write - protection) of all kernels and changes such hidden options.
-  **CsrUtil.efi**: Simply implements the SIP - related functions of Apple's csrutil.
-  **GopStop.efi**: Stops the graphics card GOP and uses a simple scenario to test the GraphicsOutput protocol for troubleshooting.
-  **KeyTester.efi**: Tests keyboard input in SimpleText mode.
-  **MmapDump.efi**: The necessity of the ProvideCustomSlide option.
-  **OpenControl.efi**: Provides NVRAM protection for other tools so that full NVRAM access can be obtained when booting from OC.
-  **OpenShell.efi**: The UEFI Shell of OpenCore configuration.
-  **ResetSystem.efi**: A utility used to perform a system reset.
-  **RtcRw.efi**: A utility used to read and write RTC (CMOS) memory.
-  **TpmInfo.efi**: Checks the Intel PTT (Platform Trust Technology) function on the platform and, if enabled, allows the use of fTPM 2.0.

Well, that's almost all for the introduction of the OpenCore file structure. Now, let's start to learn the relevant knowledge of ACPI.