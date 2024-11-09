## Hardware

The closer the hardware of a Hackintosh is to that of a genuine Mac, the smoother the Hackintosh installation process will be, and the higher the final completion rate will also be.

Due to limited time, it's inevitable that there are omissions in the information in this document. For now, the safest way is to do a lot of Google or Baidu searches before buying hardware to see if it can support Hackintosh. This can help you avoid some pitfalls.

## CPU Support

### Do the 11th - and 12th - Generation CPUs Support It?

The CPU itself can be driven. The key problem is that the integrated graphics can't work. This means you must have a discrete graphics card that doesn't require additional drivers. Because few laptops are equipped with such discrete graphics cards (and they also need to support direct connection of discrete graphics cards), so for the 11th - and 12th - Generation laptops, there is basically no solution. There are indeed a few laptops equipped with AMD CPUs and AMD discrete graphics cards, but they are not cheap. Moreover, the AMD CPUs are not perfect either, and the virtualization function doesn't work. Of course, there are many other minor problems and I won't list them one by one.

### Desktop CPUs

Take your CPU code and architecture as the standard. The following examples are for reference only. The CPU models are so chaotic that the list is not complete.

| CPU | 
| ------------ | 
| Penryn       |
| Yonah        |
| Conroe       | 
| Merom        |
| Penryn       |
| Lynnfield    |
| Clarkdale    |
| Sandy Bridge |
| Ivy Bridge   |
| Haswell      |
| Broadwell    |
| SkyLake      |
| Kaby Lake    |
| Coffee Lake  |
| Comet Lake   |

The current integrated graphics (UHD 730/750) of the 11th - generation Intel CPU can't be driven, and it's not very perfect for Hackintosh. If you don't care about the integrated graphics, you can consider directly using the 12th - generation Intel CPU, which has stronger performance. It's great to pair it with a discrete graphics card that doesn't require additional drivers:

To make the 12th - generation Core imitate the 10th - generation Core, just add the following content in config.plist → Kernel → Emulate of OpenCore:

- **Cpuid1Data**: `55060A00 00000000 00000000 00000000`
- **Cpuid1Mask**: `FFFFFFFF 00000000 00000000 00000000`

### Laptop CPU

Take your CPU code and architecture as the standard. The following examples are for reference only. The CPU models are so chaotic that the list is not complete.

| CPU 代号     |
| ------------ |
| Clarksfield  | 
| Arrandale    |
| Sandy Bridge | 
| Ivy Bridge   |
| Haswell      |
| Broadwell    |
| SkyLake      |
| Kaby Lake    |
| Whiskey Lake |
| Coffee Lake  |
| Comet Lake   |
| Ice Lake     |

### High - end Desktop CPU

It depends on the code and architecture of your CPU. The following examples are for reference only. The CPU models are in chaos, so the list is incomplete.

| CPU         |
| ---------------- |
| Nehalem          |
| Westmere         |
| Sandy Bridge-E   |
| Ivy Bridge-E     |
| Haswell-E        |
| Broadwell-E      |
| Skylake-X/W      |
| Cascade Lake-X/W |

### AMD CPU 

It depends on the code and architecture of your CPU. The following examples are for reference only. The CPU models are in chaos, so the list is incomplete.

| CPU                  |
| ------------------------- |
| Bulldozer(15h)            |
| Jaguar(16h)               |
| Ryzen                     |
| Threadripper(17h and 19h) |

The Ryzen APU laptop series can't be used basically even if Hackintosh is installed well because the discrete graphics card can't be driven. Unless your laptop is equipped with an AMD CPU and also a discrete AMD graphics card that doesn't require additional drivers (this is the case for very few and expensive devices), but the price of such laptops is usually very high. You can buy a brand - new MBP instead, so there's no need to hack.

## GPU Support

Graphics cards are also quite diverse, and it's hard to list them all completely. It's recommended to make sure there are successful cases of your graphics card model before purchasing. The graphics card driver situation can generally be referred to as follows:

- AMD GPU support situation:

  - AMD's APU series can't be driven currently.
  - AMD's discrete graphics cards have had good driver support since the GCN architecture.

- Intel GPU support situation:

  - macOS doesn't support low - end GT1 iGPUs on Pentium, Celeron, and Atom.
  - The vast majority of Intel integrated graphics can be well - driven.
  - Theoretically, the strongest integrated graphics is i7 - 1065G7, followed by the Iris series, and then the UHD and HD series.

- NVIDIA GPU support situation:

  - The Maxwell (9XX) and Pascal (10XX) series can only be installed up to macOS 10.13 High Sierra at most.
  - The Turing (20XX, 16XX) series currently has no solution.
  - The Ampere (30XX) series currently has no solution.
  - The Kepler (6XX, 7XX) series currently supports the latest version of macOS because MBP was equipped with this graphics card in history.

GPU models are also too diverse. These are not within the scope of this tutorial. The official summary of OpenCore is already very detailed. You can refer to:

[GPU Buyers Guide - Graphics Card Purchase Guide](https://dortania.github.io/GPU - Buyers - Guide/)

## Motherboard Support

The vast majority of motherboards can be driven. I still recommend choosing motherboards with more BIOS functions as much as possible because Hackintosh requires adjusting many BIOS options. Motherboards with overly simplified BIOS are not very perfect for hacking.

Here are some small tips for motherboard selection:

- Gigabyte B250M and B360M can't make the discrete and integrated graphics work perfectly, so they are not highly recommended.
  - If the BIOS is set to integrated graphics output, if your monitor's HDMI/DP is plugged into the discrete graphics card, the screen will be black until you enter the system.
  - If the BIOS is set to discrete graphics output, although you can enter the system, you will find that the integrated graphics is blocked by the motherboard, resulting in abnormal dual - hardware decoding.
  - The perfect method is to set the integrated graphics output, plug the monitor into the integrated graphics to light up the screen, enter the password to enter the system, and then plug in the discrete graphics. At this time, the dual - hardware decoding is normal (it's really a pain).
- Motherboards with the CFG Lock option in the BIOS settings are preferred, which can bring a better Hackintosh experience.
- X79, X99, X299, C422, C612, and C621 don't support macOS NVRAM reading and writing, which is also a pitfall, and it's not recommended to buy them.
- For laptop motherboards, motherboards that support the S3 sleep mode are preferred, which can make the sleep consume less power.
- Starting from the 10th - generation CPU in laptops, many motherboard manufacturers use the S0 modern sleep mode instead of S3, resulting in very imperfect power consumption during sleep.

## Hard Disk Support

All eMMC hard disks can't be driven (commonly found in some tablets or low - end laptops). Basically, all SATA hard disks are supported. A small number of NVME - protocol hard disks can't work perfectly, and may even cause kernel panics during installation. It's recommended to use all NVME hard disks with [NVMeFix.kext](https://github.com/acidanthera/NVMeFix). Here are some tips related to hard disks for your reference:

- List of hard disks with imperfect TRIM operation (can be used with TRIM disabled)
  - Samsung 950 Pro
  - Samsung 960 Evo/Pro
  - Samsung 970 Evo/Pro (there are many mysterious problems. It's recommended to block them if there are mysterious problems)

- List of hard disks with perfect TRIM operation
  - Western Digital Blue SN550
  - Western Digital Blue SN570
  - Western Digital Black SN700
  - Western Digital Black SN720
  - Western Digital Black SN750 (also called SanDisk Extreme PRO. It can be installed when the performance mode is turned off)
  - Western Digital Black SN850 (more tests are needed)
  - SK Hynix PC401 HFS512GD9TNG - 62A0A (comes with Alienware laptops)
  - ADATA XPG S11L Pro, S50 Lite, 8200 Pro. It seems that ADATA series are all good for Hackintosh.
  - SanDisk Ultra 3D NVme. It seems that SanDisk series are all good for Hackintosh.
  - Hikvision C2000, C2000 Pro series. It seems that Hikvision series are all good for Hackintosh, but the quality is a concern.
  - Lexar series, Guangwei series, Crucial series, Pioneer APS - SE20G series, YMTC AH530, Plextor M6S, Lite - On SSD

- Incompatible with IONVMeFamily or having other strange problems (the system will freeze when booting up with code running. Try not to choose them)
  - Gigabyte 512 GB GIGABYTE M.2 PCIe SSD (such as GP - GSM2NE8512GNTD)
  - ADATA Swordfish 2 TB M.2 - 2280
  - SK Hynix HFS001TD9TNG - L5B0B, HFS512GD9TNG - L2A0A, HFS512GD9TNI - L2A0B
  - SK Hynix P31, P711
  - SK Hynix BC501/PC601/PC611/PC711/BC501 (mainly found in Lenovo and Dell laptops. Some batches can't install macOS)
  - Samsung PM961/PM981/PM981a/PM991 series (MZVLB256HAHQ, etc.)
  - Samsung 983ZET, SAMSUNGKLFGGAR4AA - K0T0
  - Micron 2200V MTFDHBA512TCK, Micron 2200S
  - Intel 600P/660P/760P series (with some strange problems)
  - Kingston A2000 (needs to be paired with [NVMeFix.kext](https://github.com/acidanthera/NVMeFix)), KINGSTONSA2000M8500G
  - Asgard AN2 series
  - Asgard AN3 + (STAR1000P)
    - May also need to be paired with Kexts patch:

    ```
    IONVMeFamily   F6C1100F 85440100 00  F6C1010F 85440100 00
    ```

  - Kioxia - EXCERIA PLUS
  - Netac NVME SSD 480
  - Plextor M9P Plus PX - 512M9PGN

Although some of these models can fix kernel panics later with NVMeFix.kext, there may still be some minor problems. For beginners, it's still recommended to change hard disks and avoid these models.

## Wired Network Card

Almost all wired network adapters have some form of support in macOS, either through built - in drivers or kexts made by the community. However, there are also some tricky models:

- Intel I225 2.5Gb Network Card

  - May be equipped on high - end desktop Comet Lake motherboards
  - Possible solutions: [Source](https://www.hackintosh - forum.de/forum/thread/48568 - i9 - 10900k - gigabyte - z490 - vision - d - er - läuft/?postID = 606059#post606059) and [Example](https://dortania.github.io/OpenCore - Install - Guide/config.plist/comet - lake.html#deviceproperties)
  - There are new problems in macOS 12.3. Refer to [the 12th question in QA](https://apple.sqlsec.com/9 - %E5%B8%B8%E8%A7%81QA/#%E9%97%AE%E9%A2%98%E5%88%97%E8%A1%A8)

- Intel I350 1Gb Server Network Card

  - May be equipped on various generations of Intel and Supermicro server motherboards
  - [Solution](https://dortania.github.io/OpenCore - Install - Guide/config - HEDT/ivy - bridge - e.html#deviceproperties)

- Intel 10Gb Server Network Card

  - Possible workarounds for [X520 and X540 chipsets](https://www.tonymacx86.com/threads/how - to - build - your - own - imac - pro - successful - build - extended - guide.229353/)

- Mellanox and Qlogic Server Network Cards

## Wireless Network Card

Most of the wireless network cards that come with laptops are not supported by the native system. The best choice is the Broadcom network cards used in genuine Macs.

For more details about network cards, refer to: [WiFi Buyers Guide](https://dortania.github.io/Wireless - Buyers - Guide/) The following mainly lists the Broadcom series of network cards that don't require additional drivers:

### Broadcom PCIe Network Cards

- BCM943602CDP
- BCM943602CD
- BCM94360CD
  - Fenvi FV T919 (Bluetooth 4.0)
  - Fenvi AC1900 (no Bluetooth, End - of - Life)
  - TP - LINK Archer T9E AC1900 (no Bluetooth, End - of - Life)
  - TP - LINK Archer T8E (no Bluetooth)
  - RNX - AC1900PCE (no Bluetooth)
  - ASUS PCE - AC66 (no Bluetooth)
  - ASUS PCE - AC68 (no Bluetooth)
- BCM94331CD (may require forced kext loading in Catalina)
- BCM94360CS2
  - Fenvi FV - HB1200 (Bluetooth 4.0)
  - AWD wireless network card (no Bluetooth)
- BCM943602CS
- BCM94360CSAX
- BCM94360CS
- BCM94352Z
  - TP - LINK Archer T6 (no Bluetooth)
  - Rosewill RNX - AC1300PCE (no Bluetooth)
  - ASUS PCE - AC56 (no Bluetooth)
- BCM94350ZAE

### Broadcom Mini PCIe

- **BCM94360HMB** (ABGN + AC, BT 4.0, 3x3:3):
  - AzureWave AW - CB160H
  - Alpha Networks WMC - AC01
  - Arcadyan WN8833B - AC
  - Gemtek WMDB - 150AC
  - Unex DAXB - 81
  - Wistron NeWeb DNXB - C1
- **BCM94352HMB** (ABGN + AC, BT 4.0, 2x2:2):
  - AzureWave AW - CE123H
  - Dell DW1550
  - HP TPC - Q013
  - Lenovo Lite - On WCBN606BH

### Broadcom M.2

- BCM94360NG
  - Fenvi BCM94360NG (A + E Key, natively supported as based off of genuine Apple Airport card) (BT 4.0)
- BCM943602
  - Dell DW1830 (A + E Key, quite wide so make sure your laptop has room) (BT 4.1)
- BCM94352Z
  - Fenvi AC1200 (A + E Key, natively supported as based off of genuine Apple Airport card) (BT 4.0)
  - Dell DW1560 (A + E Key) (BT 4.0)
  - Lenovo Lite - On WCBN802B(04X6020) (E Key) (BT 4.0)
  - AzureWave AW - CB162NF (A + E Key) (BT 4.0)
- BCM94350ZAE
  - Lenovo Foxconn T77H649 (A + E Key) (BT 4.1)
  - Lite - On WCBN808B (A + E Key) (BT 4.1)
  - Dell DW1820A (A + E Key) (BT 4.1)

Later, the domestic expert [zxystd](https://github.com/zxystd) transplanted and open - sourced the Intel network card driver, which can also solve the driver problems of most Intel network cards. Although AirDrop can't be used for the time being, it's already a great improvement:

[List of drivable models of Intel network cards](https://openintelwireless.github.io/itlwm/Compat.html#dvm - iwn)

## Sound Card Support

Sound cards are mainly driven by the open - source kexts of AppleALC, which can enable native macOS high - definition audio. For a detailed list of sound cards that support native drivers, refer to:

[Driver status table and layouts id status of Hackintosh sound cards](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)

## Other Hardware

- Fingerprint Sensor

  - Currently, it's impossible to simulate the Touch ID sensor, so the fingerprint sensor basically has no solution.
- Windows Hello Face Recognition

  - Some laptops have WHFR connected by I2C (and used through your iGPU), and these will not work properly.
  - Some laptops have WHFR connected by USB. If you're lucky, you may get the camera function, but no other functions.
- Intel Smart Sound Technology

  - The microphones of laptops equipped with Intel SST usually can't be driven, such as the Xiaoxin 13 Pro series.
  - However, some laptops that also use Intel smart sound technology can drive the microphones perfectly.
  - So whether the microphone can work or not can only be known through practice.
- Headphone - Microphone Combo Interface

  - Some laptops with a combined headphone - microphone interface may not be able to get audio input through them, and must use the built - in microphone or an external audio input device via USB.
- Thunderbolt Interface

  - The Thunderbolt interface is relatively difficult to handle and somewhat mysterious. The OC official recommends disabling the Thunderbolt ports on the motherboard. But it doesn't mean there's no solution; it's just difficult.
- Card Reader

  - Some Realtek card readers can be driven perfectly. For details, refer to this project: https://github.com/0xFireWolf/RealtekCardReader

## References

- [Recommended Hardware Configuration Table for Hackintosh in 2022](https://heipg.cn/tutorial/diy - hackintosh - 2020.html) 
- https://github.com/dortania/bugtracker/issues/192
