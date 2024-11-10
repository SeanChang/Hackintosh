## Temperature Sensor

Starting from the Radeon VII, Apple stopped directly reporting temperature, and kexts are required to intervene and implement this function. For Vega 10 and earlier versions, other tools can already display the GPU temperature without an additional kext.

Address of the required item: [https://github.com/aluveitie/RadeonSensor](https://github.com/aluveitie/RadeonSensor)

It supports all GPUs from the Radeon HD 7000 series to the RX 6000 series.

First, let's take a look at several files downloaded in this project:

- **RadeonSensor.kext**: Required for reading the GPU temperature, and Lilu is needed.
- **SMCRadeonGPU.kext**: Can be optionally used to export the GPU temperature to VirtualSMC for monitoring tools to read.
- **RadeonGadget.app**: Displays the GPU temperature in the status bar, and only RadeonSensor.kext needs to be loaded.

The usage method is also relatively simple. Copy RadeonSensor.kext and SMCRadeonGPU.kext to the EFI/OC/Kexts directory, and then enable them in config.plist:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447475011422.webp) 

Reboot the computer for it to take effect.

- The GPU temperature can be normally viewed using Sensei:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447476349590.webp) 

- The GPU temperature can also be normally viewed using iStat Menus:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447477725090.webp) 

If you don't want to install SMCRadeonGPU.kext, you can also view the temperature using the included app, although it's a bit ugly:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447478651792.webp) 

## Performance Optimization

### Preface

In the era of macOS 10.15 Catalina, third - party discrete graphics cards could be disguised as professional W - series cards of genuine Macs by injecting data of genuine Macs (such as EFI Version and other information), resulting in a huge improvement in performance benchmarks. However, after macOS 11.X Big Sur, Apple officially banned this improvement method. So currently, there aren't many good ways to improve the performance of AMD discrete graphics cards that don't require drivers.

However, there's always a way out. After a large number of tests, I finally found a parameter that can significantly improve performance. Although the improvement is not as obvious as before, it still has some effect. I'll explain in detail below.

## Performance Testing

Because Geekbench5 benchmarks are too preliminary, under the same configuration, the benchmarking errors are large and have no reference value at all. So we need a stable testing environment to record the performance of graphics cards with different configurations.

### Environmental Information

Some environmental information for this test: macOS 12.2.1 + i7 - 10700 + 64GB 2667MHz memory + Sapphire RX 6600XT Platinum Edition

| Software Name | Version Information |
| -------------- | ------------------ |
| Final Cut Pro  | 10.6.1             |
| Compressor     | 4.6                |

### New Project Creation

First, click "File" - "New" - "Project" to create a new 4K 60 project. Then, in the "Subtitles and Generators" bar, select "Generator", search for the "Cloud" generator, and then drag it to the timeline:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1644900620455.webp)

### Export Testing

Because Final Cut Pro may render in the background by itself, here we select "File" - "Send to Compressor" - "New Batch":

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16449023266915.webp) 

Then select "YouTube and Facebook" - "Highest 4K" on the left and drag it to the project sent from Final Cut Pro:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-164490254283.webp) 

Finally, click "Start Batch" in the lower - right corner, and finally we can see the entire export time:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16449026554499.webp) 

The exported video is by default in the "Movies" folder under the user folder:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16449027279313.webp) 

### Final Results

| Description | 1st Time | 2nd Time | Average Time | Graphics Card Load |
| ------------ | ------- | ------- | ------------ | ------------------ |
| Imitating W6900X + Unlocking Power Limit + Overclocking Video Memory | 04 min 22 sec | 04 min 19 sec | 04 min 21 sec | 27% (Integrated graphics not working) |
| Imitating W6900X | 04 min 23 sec | 04 min 21 sec | 04 min 22 sec | 27% (Integrated graphics not working) |
| RX 6600XT + EFI Version + Scattered Information | 04 min 20 sec | 04 min 19 sec | 04 min 20 sec | 27% (Integrated graphics not working) |
| RX 6600XT Native Data | 04 min 39 sec | 04 min 41 sec | 04 min 40 sec | 19% (Integrated graphics working slightly) |
| Incorrect ROM Number, EFI + Unlocking Power Limit + Overclocking Video Memory | 04 min 43 sec | 04 min 46 sec | 04 min 45 sec | 19% (Integrated graphics working slightly) |
| RX 6600XT + Only EFI Version | 04 min 44 sec | 04 min 45 sec | 04 min 45 sec | 20% (Integrated graphics working slightly) |
| RX 6600XT + EFI Version + AAPL,slot - name | 04 min 16 sec | 04 min 14 sec | 04 min 15 sec | 22% (Integrated graphics not working) |
| RX 6600XT + AAPL,slot - name | 04 min 14 sec | 04 min 14 sec | 04 min 15 sec | 22% (Integrated graphics not working) |

The obvious difference in load occupancy:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16449437126641.webp) 

### Test Conclusions

Here are several conclusions of this test:

1. Don't randomly inject the ROM of other graphics cards into your graphics card. Even with data information of genuine Macs, the improvement is not significant.
2. When the integrated graphics participates in the export, the occupancy of the discrete graphics card will be relatively low, and the export time will be relatively long at this time.
3. Unlocking the power limit + overclocking the video memory doesn't seem to be of much use, and the export time will not be shortened as a result.
4. `AAPL,slot - name` is a key parameter. Injecting this parameter can shorten the export time. The reason is unknown. Big shots who know are welcome to leave a message.

## Final Score

In the case of the Sapphire RX 6600XT Platinum Edition + injecting AAPL,slot - name:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16454193857217.webp) 

You can directly find the path of your own graphics card in the "PCIe" tab of the Hackintool software and directly copy the device path:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16454194214545.webp) 

After this operation, the Geekbench5 benchmark score has significantly increased. The highest benchmark score is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16453636698922.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16453636752128.webp) 

Just take the Geekbench5 benchmark as entertainment. Don't take it too seriously.

Mainly, the 4K export time of Final Cut Pro + Compressor can be optimized from the slowest **04 min 41 sec** to **04 min 14 sec**, with a performance improvement of about **10%**. All in all, it's still very cost - effective.

The comments below this chapter may also be of reference value to everyone. Please pay attention and try them out by yourself.

## macOS 12.3.1

After macOS 12.3.1, many netizens reported that their graphics cards have been negatively optimized and the benchmark scores have dropped significantly. I'll also add the imitation methods for the new system here.

Just directly imitate the OCC device properties. Take the RX 5300M as an example:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517184489757.webp) 

Detailed injection parameters can refer to the details below.

### Radeon 5300

```xml
 <key>DeviceProperties</key>
    <dict>
        <key>Add</key>
        <dict>
            <key>Write the actual device path of your graphics card here (it can be seen in Hackintool PCIE)</key>
            <dict>
                <key>@0,name</key>
                <string>ATY,Keelback</string>
                <key>@1,name</key>
                <string>ATY,Keelback</string>
                <key>@2,name</key>
                <string>ATY,Keelback</string>
                <key>@3,name</key>
                <string>ATY,Keelback</string>
                <key>ATY,EFIVersion</key>
                <string>01.01.190</string>
                <key>device_type</key>
                <string>ATY,KeelbackParent</string>
            </dict>
        </dict>
    </dict>
```

### Radeon 5500

```xml
 <key>DeviceProperties</key>
    <dict>
        <key>Add</key>
        <dict>
            <key>Write the actual device path of your graphics card here (it can be seen in Hackintool PCIE)</key>
            <dict>
                <key>@0,name</key>
                <string>ATY,Python</string>
                <key>@1,name</key>
                <string>ATY,Python</string>
                <key>@2,name</key>
                <string>ATY,Python</string>
                <key>@3,name</key>
                <string>ATY,Python</string>
                <key>ATY,EFIVersion</key>
                <string>01.01.231</string>
                <key>device_type</key>
                <string>ATY,PythonParent</string>
            </dict>
        </dict>
    </dict>
```

### Radeon 5700

```xml
<key>DeviceProperties</key>
    <dict>
        <key>Add</key>
        <dict>
            <key>Write the actual device path of your graphics card here (it can be seen in Hackintool PCIE)</key>
            <dict>
                <key>@0,name</key>
                <string>ATY,Adder</string>
                <key>@1,name</key>
                <string>ATY,Adder</string>
                <key>@2,name</key>
                <string>ATY,Adder</string>
                <key>@3,name</key>
                <string>ATY,Adder</string>
                <key>AAPL00,DualLink</key>
                <data>
                AQAAAA==
                </data>
                <key>ATY,Card#</key>
                <string>102 - D32200 - 00</string>
                <key>ATY,Copyright</key>
                <string>Copyright AMD Inc. All Rights Reserved. 2005 - 2019</string>
                <key>ATY,DeviceName</key>
                <string>W5700X</string>
                <key>ATY,EFIVersion</key>
                <string>01.01.190</string>
                <key>ATY,FamilyName</key>
                <string>Radeon Pro</string>
                <key>ATY,Rom#</key>
                <string>113 - D3220E - 190</string>
                <key>CAIL_EnableLBPWSupport</key>
                <integer>0</integer>
                <key>CAIL_EnableMaxPlayloadSizeSync</key>
                <integer>1</integer>
                <key>CFG_CAA</key>
                <integer>0</integer>
                <key>CFG_FB_LIMIT</key>
                <integer>0</integer>
                <key>CFG_FORCE_MAX_DPS</key>
                <integer>1</integer>
                <key>CFG_GEN_FLAGS</key>
                <integer>0</integer>
                <key>CFG_NO_MST</key>
                <integer>0</integer>
                <key>CFG_NVV</key>
                <integer>2</integer>
                <key>CFG_PAA</key>
                <integer>0</integer>
                <key>CFG_PULSE_INT</key>
                <integer>1</integer>
                <key>CFG_TPS1S</key>
                <integer>1</integer>
                <key>CFG_TRANS_WSRV</key>
                <integer>1</integer>
                <key>CFG_UFL_CHK</key>
                <integer>0</integer>
                <key>CFG_UFL_STP</key>
                <integer>0</integer>
                <key>CFG_USE_AGDC</key>
                <integer>1</integer>
                <key>CFG_USE_CP2</key>
                <integer>1</integer>
                <key>CFG_USE_CPSTATUS</key>
                <integer>1</integer>
                <key>CFG_USE_DPT</key>
                <integer>1</integer>
                <key>CFG_USE_FBC</key>
                <integer>0</integer>
                <key>CFG_USE_FBWRKLP</key>
                <integer>1</integer>
                <key>CFG_USE_FEDS</key>
                <integer>1</integer>
                <key>CFG_USE_LPT</key>
                <integer>1</integer>
                <key>CFG_USE_PSR</key>
                <integer>0</integer>
                <key>CFG_USE_SCANOUT</key>
                <integer>1</integer>
                <key>CFG_USE_SRRB</key>
                <integer>0</integer>
                <key>CFG_USE_STUTTER</key>
                <integer>1</integer>
                <key>CFG_USE_TCON</key>
                <integer>1</integer>
                <key>PP_DisableDIDT</key>
                <integer>1</integer>
                <key>PP_DisablePowerContainment</key>
                <integer>1</integer>
                <key>PP_DisableVoltageIsland</key>
                <integer>0</integer>
                <key>PP_FuzzyFanControl</key>
                <integer>1</integer>
                <key>device_type</key>
                <string>ATY,AdderParent</string>
                <key>hda - gfx</key>
                <string>onboard - 1</string>
                <key>model</key>
                <string>Radeon Pro W5700X</string>
                <key>name</key>
                <string>ATY_GPU</string>
            </dict>
        </dict>
    </dict>
```

### Radeon 6600

```xml
<key>Write the actual device path of your graphics card here (it can be seen in Hackintool PCIE)</key>
			<dict>
				<key>@0,name</key>
				<string>ATY,Henbury</string>
				<key>@1,name</key>
				<string>ATY,Henbury</string>
				<key>@2,name</key>
				<string>ATY,Henbury</string>
				<key>@3,name</key>
				<string>ATY,Henbury</string>
				<key>ATY,DeviceName</key>
				<string>W6600X</string>
				<key>ATY,EFIVersion</key>
				<string>01.01.270</string>
				<key>ATY,FamilyName</key>
				<string>Radeon Pro</string>
				<key>device_type</key>
				<string>ATY,HenburyParent</string>
				<key>model</key>
				<string>AMD Radeon RRO W6600X</string>
				<key>name</key>
				<string>ATY,Henbury</string>
			</dict>
```

### Radeon 6800

```xml
    <key>DeviceProperties</key>
    <dict>
        <key>Add</key>
        <dict>
            <key>Write the actual device path of your graphics card here (it can be seen in Hackintool PCIE)</key>
            <dict>
                <key>@0,name</key>
                <string>ATY,Belknap</string>
                <key>@1,name</key>
                <string>ATY,Belknap</string>
                <key>@2,name</key>
                <string>ATY,Belknap</string>
                <key>@3,name</key>
                <string>ATY,Belknap</string>
                <key>device_type</key>
                <string>ATY,BelknapParent</string>
            </dict>
        </dict>
        <key>Delete</key>
        <dict/>
    </dict>
```

### Radeon 6900

```xml
    <key>DeviceProperties</key>
    <dict>
        <key>Add</key>
        <dict>
            <key>Write the actual device path of your graphics card here (it can be seen in Hackintool PCIE)</key>
            <dict>
                <key>@0,name</key>
                <string>ATY,Carswell</string>
                <key>@1,name</key>
                <string>ATY,Carswell</string>
                <key>@2,name</key>
                <string>ATY,Carswell</string>
                <key>@3,name</key>
                <string>ATY,Carswell</string>
                <key>device_type</key>
                <string>ATY,CarswellParent</string>
            </dict>
        </dict>
        <key>Delete</key>
        <dict/>
    </dict>
```

## Reference Links

The following are some reference links for writing this article. Friends who are interested can go and have a look by themselves:

- [Injection and Optimization of Asrock Radeon RX 5500 XT Challenger D 8G OC](https://github.com/huijiewei/ASRock - Z390m - ITX - ac - Opencore/blob/master/Resources/5500XT/README.md)
- [Hackintosh - AsRock Z490 Phantom Gaming ITX/TB3 - intel Core 10850k - OpenCorePkg](https://github.com/Hush - vv/Hackintosh - AsRock - Z490 - Phantom - Gaming - ITX - TB3 - intel - 10850k - OpenCorePkg)
- [Far - sighted Forum: xjn819 - Related to 6900XT and Apple's Own Monitor](https://bbs.pcbeta.com/viewthread - 1890015 - 1 - 1.html)
- [Far - sighted Forum: xjn819 - Optimization Post Exclusive to 5700xt (I've Finished Writing)](https://bbs.pcbeta.com/forum.php?mod = viewthread&tid = 1839725)
- [AMD Radeon PRO W6800X Duo and W6900X - macOS System Information](https://funnyinterestingcool.com/viewtopic.php?t = 300)