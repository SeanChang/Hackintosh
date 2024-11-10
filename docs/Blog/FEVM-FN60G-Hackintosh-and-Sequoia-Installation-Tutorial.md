 [![](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-FN60G-V2.webp)](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-FN60G - V2.webp)

## [](#fevm - fn60g黑苹果兼sequoia安装教程)FEVM FN60G Hackintosh and Sequoia Installation Tutorial

> Unconsciously, it has been ten years since I got in touch with Hackintosh. Thanks to all Hackintosh fans for accompanying me all the way. 2023 might be a year of alternation between 'black' and 'white'. Hackintosh has gradually entered its twilight years. Apple has gradually stopped fully supporting the Intel architecture. In the future, macOS might only support the arm platform for a period of time. After Sequoia, I don't know if there will be a macOS that supports the Intel architecture anymore. Everyone should cherish it while using it.

## [](#安装前准备)Pre - installation Preparation

It's already the fall of 2024. On the same day as the Mid - Autumn Festival in China, Apple released the brand - new macOS Sequoia. Both the hardware and the macOS system itself have undergone earth - shaking changes.

### [](#硬件准备)Hardware Preparation:

Before using macOS, you need to first understand the hardware limitations, that is, which hardware is supported and which is not.

Currently, all the hardware of FEVM FN60G is compatible with macOS.

#### [](#电脑配置)Computer Configuration

| Component | Model | Supported or Not |
| --- | --- | --- |
| CPU | INTEL `i9 - 14900T` / `i9 - 13900T` / `i7 - 13700T`  
/ `i5 - 13600T` / `i5 - 13400F` / `i5 - 12400F` | Supported |
| Memory | Samsung / Crucial / Micron DDR5 5600MHz | Supported |
| Integrated Graphics | Intel® Iris® Xe Graphics eligible | Not supported by macOS |
| Dedicated Graphics Card | AMD Radeon™ RX 6600M | Supported, `HDMI` x1 `4K@144Hz`  
`DP` x2 `4K@144Hz`  
`Type - C` x1 `4K@144Hz` |
| Network Card | BCM94360NG/Z3/Z4/INTEL AX2xx | Supported, Intel AX2xx does not support AirDrop |
| Bluetooth | BCM94360NG/Z3/Z4/INTEL AX2xx | Supported |
| Hard Disk 1 | PCI - E 4.0 up to 4TB  
Crucial / YMTC particles + Maxio controller | Supported |
| Hard Disk 2 | PCI - E 4.0 up to 4TB  
Crucial / YMTC particles + Maxio controller | Supported |
| USB | 10Gbps | Supported |
| Audio / 3.5mm Headphone Jack | ALC897 id:`98` | Supported |
| Audio / HDMI Output |  | Supported |
| AirDrop |  | Supported |
| Handoff |  | Supported |
| Sidecar |  | Not supported |
| Universal Control |  | Supported |

#### [](#固态硬盘)Solid - state Disk

In most cases, all SATA - based drives are supported, and most NVMe drives are also supported. There are only a few exceptions:

+   Samsung PM981(a) / PM991 and Micron 2200S NVMe SSD
    +   These solid - state disks are not compatible (resulting in kernel [crashes](https://github.com/acidanthera/NVMeFix/releases)), so [NVMeFix.kext](https://github.com/acidanthera/NVMeFix/releases) is required to fix these kernel [crashes](https://github.com/acidanthera/NVMeFix/releases). Please note that even with `NVMeFix.kext`, these drives may still cause boot problems.
    +   Related to this, the Samsung 970 EVO Plus NVMe SSD also has the same problem, but it has been fixed in the firmware update. The firmware update can be obtained [here](https://www.samsung.com/semiconductor/minisite/ssd/download/tools/) (via Samsung Magician or bootable ISO of Windows).
    +   Also note that macOS does not support laptops that use [Intel Optane Memory](https://www.intel.com/content/www/us/en/architecture - and - technology/optane - memory.html) or [Micron 3D XPoint](https://www.micron.com/products/advanced - solutions/3d - xpoint - technology) for HDD acceleration. Some users have reported success in Catalina, even with read - write support, but we strongly recommend that you remove the drive to prevent any potential boot problems.

#### [](#无线网卡)Wireless Network Card

Supported m.2 NGFF wireless network cards:

+   Broadcom:
    > Since Apple removed the driver for Broadcom network cards starting from Sonoma, Broadcom network cards are no longer driver - free. They need to be driven by patching with `OpenCore Legacy Patcher` [OCLP]. For the OCLP driving tutorial, [please go here](https://blog.daliansky.net/OCLP.html).
    Due to space limitations, the available models include: `BCM94360Z3` / `BCM94360Z4` / `BCM94352Z`. Due to the space of the network card, it also does not support original Apple disassembled cards, etc.;
+   INTEL:
    Thanks to the [OpenIntelWireless](https://openintelwireless.github.io/General/Installation.html) developed by the [@zxystd](https://github.com/zxystd) team.

### [](#软件准备)Software Preparation

#### [](#操作系统)Operating System:

An operating system that can create an installation USB disk, including but not limited to macOS / Windows / Linux, etc.

For example:

+   Apple computers running macOS;
+   Computers running Windows or PE;
+   Linux systems running in Live CD mode, etc.;

#### [](#软件或者用到的工具)Software or Tools Used:

##### [](#md5检查器)md5 Checker:

+   Windows:
    +   [WinMD5](https://www.winmd5.com/)
+   macOS or Linux built - in:
    +   `md5` for macOS
    +   `md5sum` for linux

##### [](#磁盘分区工具)Disk Partitioning Tool

+   Windows:
    +   [Disk Genuis](https://www.diskgenius.cn/)
+   macOS or Linux:
    +   omitted

##### [](#u盘制作工具)USB Disk Creation Tool

+   [etcher](http://etcher.io/)
+   [transmac](https://www.acutesystems.com/scrtm.htm)

#### [](#创建usb安装盘)Create a USB Installation Disk

##### [](#下载安装镜像)Download the Installation Image

+   Download from this site: [please click to go](https://blog.daliansky.net/categories/%E4%B8%8B%E8%BD%BD/%E9%95%9C%E5%83%8F/)

##### [](#校验md5值)Verify the md5 Value

+   In the Windows environment:
    Use the [WinMD5](https://www.winmd5.com/) downloaded just now to check whether the md5 value is correct. If the md5 values are different, **you must redownload the installation image. Don't take chances**.
+   In the macOS environment:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line"><span class="comment"># md5 macOS\ Sequoia\ 15.0</span></span><br></pre></td></tr></tbody></table>

##### [](#将安装镜像写到usb上制作安装镜像)Write the Installation Image to the USB (Create the Installation Image)

+   Image creation:
    +   Download [balenaEtcher](https://etcher.io/), select the installation image, select the USB disk to be created, and click `Flash`. ***Windows 10 needs to be run with administrator privileges***

#### [](#查找适合自己的efi)Find the EFI Suitable for You

+   This site: [Hackintosh Hackintosh Long - term Maintenance Model Checklist](https://blog.daliansky.net/Hackintosh-long-term-maintenance-model-checklist.html)
+   Others:
    +   PCbeta: [http://bbs.pcbeta.com](http://bbs.pcbeta.com/)
    +   tonymacx86: [https://www.tonymacx86.com](https://www.tonymacx86.com/)
    +   insanelymac: [insanelymac.com](https://www.insanelymac.com/)
    +   Google: [https://www.google.com](https://www.google.com/)

#### [](#替换usb安装盘里的efi)Replace the EFI in the USB Installation Disk

If the EFI that comes with the USB installation disk cannot complete the installation or the installation is not perfect, then you need to perform the operation of replacing the EFI.

+   Operation process: (omitted)

## [](#安装sequoia)Install Sequoia

### [](#bios-设置)BIOS Settings

Since the default factory BIOS of FN60G has been optimized for macOS, there is no need to set the BIOS options. This part is omitted.

### [](#安装macos-sequoia)Install macOS Sequoia

Turn on the computer, press F7 to select USB disk boot, move the cursor to `EFI USB Device` and select the `OpenCore` partition to start:

![Sequoia_OC](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia_OC.webp)

Enter the main boot interface of `OpenCore`, select `Install macOS Sequoia`, and directly press Enter to enter the `OpenCore` boot. During this period, the boot log will be displayed, which is the common `-v` (verbose mode). If unfortunately it gets stuck, please take a picture and send it to the QQ group for help. You can also go to: [Common Problems and Solutions in macOS BigSur 11.0 Installation](https://blog.daliansky.net/Common - problems - and - solutions - in - macOS - BigSur - 11.0 - installation.html); those who don't know how to operate `OpenCore` please study in advance: [In - depth Explanation of OpenCore](https://blog.daliansky.net/OpenCore - BootLoader.html)

![Sonoma_Installer_03](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sonoma/Sonoma_Installer_02.webp)

Many users will encounter problems at this point. If there are problems, please enter the group to report and provide pictures of the problem and the machine configuration diagram. **Asking questions without providing any information is irresponsible**.

![Sequoia-Installer_003](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_003.webp)

This process takes 1 - 2 minutes. Wait patiently and enter the installation program.

The installation interface appears. Select `Disk Utility` and click `Continue`.

![Sequoia-Installer_004](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_004.webp)

Enter `Disk Utility`, click as shown in the figure below, and select `Show All Devices`.

![Sequoia-Installer_005](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_005.webp)

> ***The operations performed in `Disk Utility` involve your data security. Please confirm carefully before operating. Otherwise, this site will not be responsible for all consequences caused thereby.***

![Sequoia-Installer_006](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_006.webp)

In the figure, directly erase the container instead of the entire disk. It can retain the EFI content previously copied into the EFI partition; ***In this example, `container disk2` is the name of the container to be installed. You can also click the `AMD Ventura volume` in the row below `container disk2`, and then click `Erase` in the upper - right corner. Please select the corresponding disk according to your device***

![Sequoia-Installer_007](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_007.webp)

Click `Erase`, enter in the pop - up window: Name: `Sequoia`; Format: `APFS`;

> ***Assume that your disk is empty or the data has been backed up. Don't blame me for not reminding you!!!***

![Sequoia-Installer_008](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_008.webp)

Click `Erase`, then wait for the operation to end, click `Done`, and either choose `Quit Disk Utility` from the menu or press the red button in the upper - left corner of the window to leave the disk utility.

![Sequoia-Installer_009](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_009.webp)

Return to the installation interface, select `Install macOS Sequoia`, and click `Continue`.

![Sequoia-Installer_010](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_010.webp)

Click `Agree` to continue.

![Sequoia-Installer_011](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_011.webp)

Read the terms of the license agreement and click `Agree`.

![Sequoia-Installer_012](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_012.webp)

I have read and agreed to the terms of the software license agreement, click `Agree`.

![Sequoia-Installer_013](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_013.webp)

Select the disk volume label `Sequoia` to be installed and click `Continue`.

![Sequoia-Installer_014](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_014.webp)

It will pre - copy the installation files on the USB installation disk to the system partition to be installed. This process usually lasts 1 - 2 minutes. When there are about 12 minutes remaining, the system will automatically restart and enter the second stage of installation.

![Sequoia-Installer_015](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_015.webp)

Continue the installation after restarting. During the installation process, it usually restarts automatically 3 - 4 times.

![Sequoia-Installer_016](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_016.webp)

![Sequoia-Installer_017](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_017.webp)

![Sequoia-Installer_018](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_018.webp)

![Sequoia-Installer_019](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_019.webp)

![Sequoia-Installer_020](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_020.webp)

The installation time of Sonoma is usually twice that of Catalina. It also depends on the read - write speed of the solid - state disk. Please be patient. After the installation is completed, the setup wizard will be entered.

Select `Country/Region`: `Mainland China`, and click the `Continue` button.

![Sequoia-Installer_021](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_021.webp)

Set the keyboard, use the default values, and click the `Continue` button.

![Sequoia-Installer_022](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_022.webp)

Enter the accessibility settings. By default, do not set and choose `Later` to continue.

![Sequoia-Installer_023](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_023.webp)

Enter the network connection settings and click `Other Network Options`.

[Screenshot omitted]

> It is especially important to remind that: when the Hackintosh setup wizard is running, do not connect to the network, and do not log in to the `Apple ID`. You can log in to the `Apple ID` after entering the desktop.

A prompt message pops up: `My computer is not connected to the Internet`, click the `Continue` button.

The `Accessibility` option appears, click `Later` to continue.

![Sequoia-Installer_024](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_024.webp)

The `Data & Privacy` option appears. After reading, click the `Continue` button.

![Sequoia-Installer_025](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_025.webp)

The `Migration Assistant` appears. If it's a fresh installation and you are not using `Time Machine` to restore data, please click `Later` to continue.

![Sequoia-Installer_026](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_026.webp)

The `Sign in with your Apple ID` option appears. Please select `Set up later`.

![Sequoia-Installer_027](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_027.webp)

Select `Skip` in the pop - up window.

The `Terms and Conditions` appear. Please read them and then click `Agree` to continue.

![Sequoia-Installer_028](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_028.webp)

Click `Agree` again in the pop - up window to continue.

![Sequoia-Installer_029](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_029.webp)

The window for creating a user account appears. Enter the username and password, and click the `Continue` button.

![Sequoia-Installer_030](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_030.webp)

The `Enable Location Services` window appears. Click the `Continue` button.

![Sequoia-Installer_031](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_031.webp)

The analysis window appears. Uncheck `Share Mac analysis with Apple` and click the `Continue` button.

![Sequoia-Installer_032](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_032.webp)

The screen time window appears. Click `Set up later` to continue.

![Sequoia-Installer_033](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_033.webp)

The Siri setup interface appears. Click the `Continue` button.

![Sequoia-Installer_034](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_034.webp)

Select the Siri language and click the `Continue` button.

![Sequoia-Installer_035](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_035.webp)

Enter the `Select Siri Voice` interface. Select `Voice 1` and click the `Continue` button.

![Sequoia-Installer_036](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_036.webp)

Enter the Siri improvement and dictation interface. Select `Later` and click the `Continue` button.

![Sequoia-Installer_037](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_037.webp)

A pop - up interface appears, allowing you to choose the appearance.

![Sequoia-Installer_038](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_038.webp)

You can choose the light - colored theme or the dark - colored theme according to your personal preference, and click the `Continue` button.

The `Welcome to MAC` interface appears. Click `Continue`.

![Sequoia-Installer_039](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_039.webp)

The setup wizard is completed. Depending on the selected theme, enter different interfaces respectively.

After the desktop appears, the `Keyboard Setup Assistant` may pop up. Click the `Quit` button directly. In this way, the entire installation wizard is completed.

![Sequoia-Installer_040](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Sequoia-Installer_040.webp)

## [](#安装后的系统设置)Post - installation System Settings

After the system is installed, you can have a cup of coffee and get excited for a while. There are more arduous tasks waiting for you right away.

First, open the terminal and enter the command:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">sudo spctl --master - disable        # Enable macOS to install applications from any source</span><br></pre></td></tr></tbody></table>

Enter the user password **Note: The password will not be displayed on the screen**.

> The purpose is to allow the installation of macOS applications downloaded from third - party sources.

Continue to enter the command:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">sudo rm - rf /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist*</span><br></pre></td></tr></tbody></table>

> The purpose is to clear the network devices and re - order them as: `en0` / `en1` / `en2`*, so that you can log in to the `app store` smoothly.

Continue to enter the command:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br></pre></td><td class="code"><pre><span class="line">sudo pmset - b hibernatemode 0        # Memory power supply, memory mirroring is not written to the hard disk</span><br><span class="line">sudo pmset - b acwake 0                # Turn off being awakened by devices under the same iCloud</span><br></pre></td></tr></tbody></table>

> The purpose is to optimize power management and solve the problem of unable to wake up from hibernation.

Continue to enter the command:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">sudo reboot</span><br></pre></td></tr></tbody></table>

Reboot the system.

I have made a one - click execution script for the above commands. If the network connection is smooth, you can directly execute it:

Open the terminal and enter the command:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">$ sh - c "<$(curl - fsSL https://mirror.ghproxy.com/https://raw.githubusercontent.com/daliansky/morefine - S500 - Hackintosh/main/Tools/morefine - s500.sh)>"</span><br></pre></td></tr></tbody></table>

Execution result:

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br></pre></td><td class="code"><pre><span class="line">Executing the Hackintosh one - click optimization script. The script includes:</span><br><span class="line">Enable macOS to install applications from any source / Fix sleep wake - up / Fix unable to log in to the App Store;</span><br><span class="line">Please enter the password of the current user and then press Enter</span><br><span class="line">Password:</span><br><span class="line">The Hackintosh one - click optimization script has been executed. Please restart the machine!</span><br></pre></td></tr></tbody></table>

### [](#将u盘中的efi复制进硬盘)Copy the EFI in the USB Disk to the Hard Disk

#### [](#工具篇)Tool Section

The purpose is to use macOS without USB disk boot, so it is the top - priority action.

The simplest method: Use the tool `Hackintool`. [Download the Hackintool tool](https://github.com/headkaze/Hackintool/releases) As shown in the figure:

1. Open the `Hackintool` tool and click the `Disk` icon.  
   ![Hackintool_Disk](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Hackintool_Disk.webp)

2. Click the mount icon and enter the user password.  
   ![Hackintool_Disk](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Hackintool_Disk_Mount.webp)

3. Click to mount the EFI partitions of the solid - state disk and the installation USB disk respectively, and open the folders.  
   ![Hackintool_Disk](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Hackintool_Disk_Mount2.webp)

4. Copy the `EFI` directory in the EFI partition of the USB disk to the EFI partition of the solid - state disk.  
   ![EFI Copy](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-EFI_Copy.webp)

#### [](#命令行篇)Command - line Section

##### [](#查看磁盘分区表)View the Disk Partition Table

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">diskutil list</span><br></pre></td></tr></tbody></table>

/dev/disk0(internal, physical):

| #: | TYPE | NAME | SIZE | IDENTIFIER |
| --- | --- | --- | --- | --- |
| 0: | GUID\_partition\_scheme |  | 2.0 TB | disk0 |
| 1: | EFI | B550 | 200 MB | disk0s1 |
| 2: | Apple\_APFS | Container disk1 | 2.0 TB | disk0s2 |

/dev/disk3(external, physical):

| #: | TYPE | NAME | SIZE | IDENTIFIER |
| --- | --- | --- | --- | --- |
| 0: | GUID\_partition\_scheme |  | 15.5 GB | Disk3 |
| 1: | EFI | EFI | 200 MB | disk3s1 |
| 2: | Apple\_HFS | Install macOS Ventura | 14.1 GB | Disk3s2 |
| 3: | Microsoft Basic Data | CLOVER | 299.9MB | Disk3s3 |
| 4: | Microsoft Basic Data | PE | 798.0MB | Disk3s4 |

##### [](#挂载固态硬盘efi分区)Mount the EFI Partition of the Solid - state Disk

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">sudo diskutil mount disk0s1</span><br></pre></td></tr></tbody></table>

##### [](#挂载u盘efi分区)Mount the EFI Partition of the USB Disk

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">sudo diskutil mount disk3s1</span><br></pre></td></tr></tbody></table>

Open Finder, note that there is a `.` after it.

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">open.</span><br></pre></td></tr></tbody></table>

Two EFI partitions will be shown on the left. Copy the entire EFI directory of the USB disk to the EFI partition of the disk.

![EFI Copy](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-EFI_Copy.webp)

### [](#如何更新-efi)How to Update the EFI

> The new EFI will also fix the problems that occur when updating the `OpenCore` version, and will also continuously provide support for future macOS versions.

#### [](#更新-efi)Update the EFI

Use the tool `Hackintool`. [Download the Hackintool tool](https://github.com/headkaze/Hackintool/releases) As shown in the figure:

1. Download the `Intel NUC9 EFI`
    + GitHub repository: [https://github.com/daliansky/Intel-NUC9-Hackintosh](https://github.com/daliansky/Intel-NUC9-Hackintosh)
2. Open the `Hackintool` tool and click the `Disk` icon.

![Hackintool_Disk](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Mount_EFI.webp)

3. Remove the old `EFI` directory.

![Delete EFI](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-Remove_Trash.webp)

4. Copy the downloaded `EFI` directory to the EFI partition of the solid - state disk.

![EFI Copy](https://seanchang.github.io/picx-images-hosting/20241110/hackintosh.tools-EFI_Copy2.webp)

## [](#acknowledgments)Acknowledgments

+ macOS of [Apple](https://www.apple.com/)
+ Projects maintained by [RehabMan](https://github.com/rehabman): [OS - X - Clover - Laptop - Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config), [Laptop-DSDT-Patch](https://github.com/RehabMan/Laptop-DSDT-Patch), [OS - X - USB - Inject - All](https://github.com/RehabMan/OS-X-USB-Inject-All), etc.
+ Projects maintained by [Acidanthera](https://github.com/acidanthera): [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg), [lilu](https://github.com/acidanthera/Lilu), [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup), [WhateverGreen](https://github.com/acidanthera/WhateverGreen), [VirtualSMC](https://github.com/acidanthera/VirtualSMC), [AppleALC](https://github.com/acidanthera/AppleALC), [BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM), [MaciASL](https://github.com/acidanthera/MaciASL), etc.
+ Tools provided by [headkaze](https://www.insanelymac.com/forum/profile/1364628 - headkaze/): [hackintool](https://github.com/headkaze/Hackintool), [PinConfigurator](https://github.com/headkaze/PinConfigurator), [BrcmPatchRAM](https://www.insanelymac.com/forum/topic/339175-brcmpatchram2-for-1015-catalina-broadcom-bluetooth-firmware-upload/)
+ Projects maintained by [CloverHackyColor](https://github.com/CloverHackyColor): [CloverBootloader](https://github.com/CloverHackyColor/CloverBootloader), [CloverThemes](https://github.com/CloverHackyColor/CloverThemes)
+ Projects maintained by [ic005k](https://github.com/ic005k/): [OpenCore Auxiliary Tools OpenCore Configurator OCAT](https://github.com/ic005k/QtOpenCoreConfig)
+ Organized by Xianwu: [P - little](https://github.com/daliansky/P - little), [OC - little](https://github.com/daliansky/OC-little)
+ Projects maintained by [chris1111](https://github.com/chris1111): [VoodooHDA](https://github.com/chris1111/VoodooHDA - 2.9.2 - Clover - V15), [Wireless USB Adapter Clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover)
+ [itlwm](https://github.com/zxystd/itlwm) and [IntelBluetoothFirmware](https://github.com/zxystd/IntelBluetoothFirmware) developed by [zxystd](https://github.com/zxystd)
+ Tools provided by [lihaoyun6](https://github.com/lihaoyun6): [CPU - S](https://github.com/lihaoyun6/CPU - S), [macOS - Displays - icon](https://github.com/lihaoyun6/macOS-Displays-icon), [SidecarPatcher](https://github.com/lihaoyun6/SidecarPatcher)
+ Tools provided by [xzhih](https://github.com/xzhih): [one - key - hidpi](https://github.com/xzhih/one - key - hidpi)
+ [In - depth Explanation of OpenCore](https://blog.daliansky.net/OpenCore - BootLoader.html) updated and maintained by [Bat.bat](https://github.com/williambj1)
+ [OpenCore 0.5 + Component Patches](https://blog.cloudops.ml/ocbook/) and [Common - patches - for - hackintosh](https://github.com/athlonreg/Common-patches-for-hackintosh) updated and maintained by [athlonreg](https://github.com/athlonreg)
+ [XiaoMi NoteBook Pro Hackintosh](https://github.com/daliansky/XiaoMi-Pro-Hackintosh) and [one-key-cpufriend](https://github.com/stevezhengshiqi/one-key-cpufriend) updated and maintained by [stevezhengshiqi](https://github.com/stevezhengshiqi)
+ Configuration files of various platforms organized by [Miracle - Sakuno](https://github.com/Miracle-Sakuno)
+ [github.com](https://blog.daliansky.net/github.com)
+ [gitee.io](https://blog.daliansky.net/gitee.io)
+ [coding.net](https://blog.daliansky.net/coding.net)

## [](#references-and-citations)References and Citations:

+ [https://deviwiki.com/wiki/Dell](https://deviwiki.com/wiki/Dell)
+ [https://deviwiki.com/wiki/Dell_Wireless_1820A_(DW1820A)](https://deviwiki.com/wiki/Dell_Wireless_1820A_(DW1820A))
+ Broadcom 4350 updated by [Hervé](https://blog.daliansky.net/[https://osxlatitude.com/profile/4953-herv%C3%A9/](https://osxlatitude.com/profile/4953-herv%C3%A9/)): [https://osxlatitude.com/forums/topic/12169-bcm4350-cards-registry-of-cardslaptops-interop/](https://osxlatitude.com/forums/topic/12169-bcm4350-cards-registry-of-cardslaptops-interop/)
+ DW1820A - supported model list updated by [Hervé](https://blog.daliansky.net/[https://osxlatitude.com/profile/4953-herv%C3%A9/](https://osxlatitude.com/profile/4953-herv%C3%A9/)): [https://osxlatitude.com/forums/topic/11322-broadcom-bcm4350-cards-under-high-sierramojave/](https://osxlatitude.com/forums/topic/11322-broadcom-bcm4350-cards-under-high-sierramojave/)
+ Bluetooth driver provided by [nickhx](https://osxlatitude.com/profile/129953 - nickhx/): [https://osxlatitude.com/forums/topic/11540-dw1820a-for-7490-help/?do=findComment&comment=92833](https://osxlatitude.com/forums/topic/11540-dw1820a-for-7490-help/?do=findComment&comment=92833)
+ [Using OpenCore to Boot Hackintosh](https://blog.xjn819.com/?p = 543) and [The Correct Way to Use AptioMemoryFix.efi for 300 - series Motherboards (Revised Version)](https://blog.xjn819.com/?p=317) by [xjn819](https://blog.xjn819.com/)
+ [insanelymac.com](https://www.insanelymac.com/)
+ [tonymacx86.com](https://www.tonymacx86.com/)
+ [bbs.pcbeta.com](http://bbs.pcbeta.com/)
+ [applelife.ru](https://applelife.ru/)
+ [olarila.com](https://www.olarila.com/)
+ [Hackintoshlifeit](https://github.com/Hackintoshlifeit)