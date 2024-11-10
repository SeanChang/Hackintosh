If the integrated graphics are used for output under macOS, generally the DP interface is driver - free. If HDMI is used, the monitor may be black or the color output may be incorrect. So we often need to customize the display interface.

## Device Introduction

The device information used in this tutorial is as follows:

| Category | Value |
| ---- | ---- |
| Motherboard | AsRock Z490 Steel Legend |
| CPU | i7 - 10700 |
| GPU | Intel UHD Graphics 630 |
| Interface | DP 1.4 + HDMI 1.4 |

## OC Configuration Information

To be concise, since my motherboard basically has all options, only the ID attribute is injected for the integrated graphics part here:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517610683725.webp) 

After restarting and entering the system, Hackintool automatically determined the current platform ID as `0x3E9B0007` through this integrated graphics ID.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517611719856.webp) 

## View Interface Information

In Hackintool, find "Applied Patches" - "Interfaces", select the platform ID you saw above, and you can see the current interface situation:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517624967275.webp) 

It can be seen that this platform has three interfaces by default, and all of them are DP. However, the interfaces of our motherboard are as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517628118959.webp) 

It is a classic combination of 1 HDMI + 1 DP. So at this time, our HDMI has no signal display. So what can we do to make this HDMI display? Don't worry. Keep reading.

## Display Interface Types

- **02000000**
  - LVDS and eDP interfaces
  - Generally used to drive the built - in monitor of a laptop
- **00040000**
  - DP interface
  - Generally, the monitor built in the USB - C is a DP interface.
  - The DP interface is usually driver - free under macOS.
- **00080000**
  - HDMI interface
  - HDMI under macOS usually cannot display normally and the interface data needs to be customized.
- **10000000**
  - VGA interface
  - VGA monitors are not supported by default after version 10.8.
  - If your CPU is from the 6th generation onwards, it actually uses the DP interface, so just drive it as a DP interface.
- **04000000**
  - Dual - link DVI interface
  - This thing is basically obsolete.
- **00020000**
  - Single - link DVI interface
  - This thing is basically obsolete.
- **80000000**
  - S - Video interface
  - This thing is basically obsolete.
- **01000000**
  - Used when there is no physical display interface.

## Look for the Official Manual

Look for the buffer frame information of our current platform ID in [WhatGreen's official manual](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md):

```ini
Model name: Intel UHD Graphics 630         ID: 3E9B0007
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[3] busId: 0x06, pipe: 8, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
01050900 00040000 C7030000
02040A00 00040000 C7030000
03060800 00040000 C7030000
```

It can be seen that three bus IDs are listed for the 3E9B0007 platform. We will decompose them for easier understanding. Here, take the first one as an example. For a detailed explanation, see the figure below:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517674262548.webp) 

- The "bus ID" **is unique, and multiple interfaces cannot share it**.
- The maximum value of the "bus ID" on most platforms is `06`.
- The "bus ID" is crucial. Generally, when we customize the interface, we are actually exploring and traversing this value.
- The "channel" actually has no reference value. You can write it according to the values recommended for different platforms in the manual.
- `C7030000` is an identifier. Fill it in according to the manual.
- Customizing the number of interface displays also requires the use of `framebuffer - patch - enable | Data | 01000000`.

## Traverse Interface 1

Now start customizing display interface 1. Generally, only the busid needs to be traversed. The index number, channel, and identifier remain the same as those queried in the manual. Specify interface 1 as the HDMI type:

```ini
framebuffer - con0 - alldata = 01010900 00080000 C7030000
framebuffer - con0 - enable  = 01000000
```

The final effect is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1651803578171.webp) 

After replacement, restart and take a look. Although HDMI doesn't work, it can be seen that the specified HDMI interface type we set has taken effect:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16518039361008.webp) 

Since using "bus ID" `01` for interface 1 is not successful, start trying to traverse it to the maximum value `06` one by one:

```ini
framebuffer - con0 - alldata = 01020900 00080000 C7030000
framebuffer - con0 - alldata = 01030900 00080000 C7030000
framebuffer - con0 - alldata = 01040900 00080000 C7030000 (Since interface 2 is also bus ID 4, remember to unplug the DP)
framebuffer - con0 - alldata = 01050900 00080000 C7030000
framebuffer - con0 - alldata = 01060900 00080000 C7030000
```

Restart after each traversal. At most, only 5 more restarts are needed. If none of them work, it means that this "interface 1" doesn't correspond to the physical HDMI interface at all. Then only interface 3 can be traversed.

## Traverse Interface 3

Enable interface 3, specify the display type as HDMI, and start traversing from "bus ID" `01`. At most, try restarting the computer 6 times:

```ini
framebuffer - con2 - alldata = 03010800 00080000 C7030000
framebuffer - con2 - alldata = 03020800 00080000 C7030000
framebuffer - con2 - alldata = 03030800 00080000 C7030000
framebuffer - con2 - alldata = 03040800 00080000 C7030000 (Since interface 2 is also bus ID 4, remember to unplug the DP)
framebuffer - con2 - alldata = 03050800 00080000 C7030000
framebuffer - con2 - alldata = 03060800 00080000 C7030000
```

Fortunately, we finally succeeded when we tried "bus ID" `04`:

```ini
framebuffer - con2 - alldata = 03040800 00080000 C7030000
```

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16518045207212.webp) 

Since our interface 2 also uses "bus ID" `04`, if both DP and HDMI are plugged in at this time, neither monitor can display normally. So the DP cable, that is, interface 2, must be unplugged for testing. Finally, it is found that the HDMI interface successfully lights up the monitor:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16518044336845.webp) 

However, at this time, due to the conflict of "bus ID" between interface 2 and 3, we have to manually change interface 2 to another "bus ID". All of them also need to be traversed:

```ini
framebuffer - con1 - alldata = 02010A00 00040000 C7030000
framebuffer - con1 - alldata = 02020A00 00040000 C7030000
framebuffer - con1 - alldata = 02030A00 00040000 C7030000
framebuffer - con1 - alldata = 02050A00 00040000 C7030000
framebuffer - con1 - alldata = 02060A00 00040000 C7030000
```

Fortunately, we finally succeeded when we tried "bus ID" `02`:

```ini
framebuffer - con1 - alldata = 02020A00 00040000 C7030000
```

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16518056893408.webp) 

Also, remember that for multiple monitors, use the boot option `igfxonln = 1`.

## Final Effect

Finally, after restarting the computer nearly 10 times, the DP and HDMI interfaces can finally light up the monitors simultaneously:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16518062926032.webp) 

## Teach One to Fish

It is guessed that there are actually rules for these motherboards of various motherboard manufacturers. So some common combination cases can be placed below this article. Since my energy is limited, everyone is also welcome to share your successful cases in the comment section (just attach the motherboard model and interface data). The specific format can refer to the following:

### AsRock

- **DP + HDMI**: Z490 Steel Legend

```ini
framebuffer - con1 - alldata = 02020A00 00040000 C7030000
framebuffer - con2 - alldata = 03040800 00080000 C7030000 
```

- **DP + HDMI**: Z490 Extreme4

```ini
framebuffer - con2 - alldata = 03040800 00080000 C7030000
```

### ASUS

- **DP + HDMI**: ROG STRIX B460 - I GAMING

```ini
framebuffer - con0 - alldata = 01060900 00040000 C7030000
framebuffer - con1 - alldata = 02040A00 00080000 C7030000
```

- **DP + HDMI**: Z170I PRO GAMING

```ini
framebuffer - con0 - alldata = 01050900 00080000 C7030000 
```

- **DP + HDMI**: TUF GAMING B460M - PRO

```ini
framebuffer - con1 - alldata = 02060A00 00080000 C7030000 
```

- **HDMI + DVI**: TUF B360M - PLUS GAMING

```ini
framebuffer - con1 - alldata = 02020A00 00080000 C7030000 
```

- ASUS TUF Gaming FX504GE HM370 chipset

```ini
framebuffer - con2 - alldata =01050900 00080000 C7010000
```

### Dell

- **HDMI + VGA**: Vostro 3260 100 - series

```ini
AAPL,ig - platform - id = 00001259
framebuffer - con0 - alldata = 01050900 00080000 C7030000
framebuffer - con1 - alldata = 02000A00 00040000 C7030000 （VGA may not work）
```

### MSI

- **HDMI**: MAG B365M MORTAR 

```ini
framebuffer - con0 - alldata = 01050900 00080000 C7030000
```

- **DP + HDMI**: MPG Z390 GAMING PRO CARBON AC

```ini
framebuffer - con0 - alldata = 01010900 00080000 C7030000 
```

- **DP + HDMI**: MAG B460M MORTAR

```ini
framebuffer - con0 - alldata = 01010800 00080000 C7030000
```

- **DP + HDMI + Thunderbolt**: MEG Z490I UNIFY

```ini
framebuffer - con0 - alldata = 01010900 00080000 C7030000
```

### Lenovo

- **HDMI**: Yangtian S540 - 14 - IWL - HDMI

```ini
framebuffer - con1 - alldata = 01010900 00080000 C7010000
```

- **HDMI + VGA**: V330 - 15IKB LNVNB161216 chipset

```ini
framebuffer - con1 - alldata = 01050900 00040000 87010000
framebuffer - con2 - alldata = 02040A00 00080000 87010000
```

### Gigabyte

- **DP + HDMI**: B360M AORUS PRO

```ini
framebuffer - con0 - alldata = 01050900 00040000 C7030000
framebuffer - con2 - alldata = 03040800 00080000 C7030000
```