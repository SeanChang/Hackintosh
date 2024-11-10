??? + question "1. Wake from sleep with black screen or need to plug and unplug the monitor cable to turn on the screen and enter the system during boot."
    Try adding the `igfxonln = 1` parameter to the boot options, and you can also try adding the `gfxrst = 1` parameter to the boot options.

??? + question "2. My graphics card is driver - free, but the system boots to a black screen with no output signal."
    Try adding the `agdpmod = pikera` parameter to the boot options. This can be used for new driver - free series graphics cards such as RX5500/5600/5700/6600/6800/6900 to prevent black screens during the boot process.

??? + question "3. Laptop wakes from sleep with black screen."
    There are many possibilities for this situation. One possibility is that the discrete graphics card is not masked. Try adding the `-wegnoegpu` parameter to the boot options.

??? + question "4. When installing the system, it prompts 'An Internet connection is required to install macOS'."
    Some friends in the group have encountered this problem. The solution is: just connect the network cable. It's really as the name implies.

??? + question "5. What if macOS always fails to detect system updates?"
    Open OCC, under the 「Misc - Other Settings」 - 「Security」 tab, change **SecureBootMode** to **Default**.

??? + question "6. After the integrated graphics finishes buffering frames, HEVC decoding doesn't work, and REQ is only up to 0.35Ghz at most."
    For the integrated graphics device in the DeviceProperties device attribute settings, delete `AAPL, slot - name`.

??? + question "7. When booting, if it prompts [oc grabbed zero systm - id for sb. this is not allowed halting on critlcal error]"
    Basically, it's a problem with 【SecureBootModel】 under 【Misc】 --> 【security】. The default 【Default】 can be changed to 【Disabled】 or other options.

??? + question "8. When booting, if it gets stuck at the 【End SetConsoleMode】 error."
    Basically, it's a problem with 【SecureBootModel】 under 【Misc】 --> 【security】. The default 【Default】 can be changed to 【Disabled】 or other options.

??? + question "9. After waking from sleep, there are inexplicable screen - glitching phenomena."
    Try injecting more video memory into the integrated graphics properties, such as 2048MB `framebuffer - unifiedmem 00000080` of data type.

??? + question "10. Can't find the disk partition where macOS has been installed."
    Use OCC to apply a `Fix RTC _STA bug` patch in the ACPI options, or if your OCC version is higher than the installed system version, change 「MinVersion」 to 「- 1」 (unlimited) in 「UEFI Settings」 - 「Embedded APFS」.

??? + question "11. When installing the system, it prompts: 'Installation cannot continue because the installer is damaged'."
    There are two possibilities:

    1. As the name implies, the installation image is really damaged. The solution is to change to another image and burn and install again. (This possibility is not high.)
    2. The current time is incorrect. Open the terminal and enter date to see if the time is correct. If it's incorrect, use the date command to change the time.

??? + question "12. I enter the system, and after a few minutes, it freezes, blacks out, and restarts. It's normal without plugging in the network cable. The 1225V network card doesn't work properly."
    First, make sure the path of your network card is correct, and then the way of driving is correct. The following two are the key parameters: 

   ![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-165407829091.webp) 
    Then, starting from macOS12.3, the boot option parameter has changed from the previous `dk.e1000 = 0` parameter to adding the `e1000 = 0` parameter. So, if it's incorrect, replace or add it.

??? + question "13. USB works normally without customization, but if customized using USBToolBox, it will directly get stuck at APFS and can't enter the operating system."
    This may have occurred on some USB3.1 devices such as ASMedia ASM1142. When [customizing USB](/6 - Practical Postures/6 - 1/), don't plug in this interface, and then choose `I` (ignore) at the following step:

   ![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16494854148280.webp)

??? + question "14. The App Store of macOS 10.13.6 can't be used, and the download prompt is 'Try again from the Purchased page'."
    Actually, the App Store of 10.13.6 is too old. Update the browser and iTunes, and these mysterious problems can be solved:

   ![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1649485644685.webp)

??? + question "15. The ASMedia ASM1142 USB 3.1 Type - A and Type - C integrated interface doesn't work."
    Use this [SSDT - USB3 - 1 - XHC2.aml](https://sqlsec.lanzoub.com/iWZDt02w295g) SSDT to solve the problem.

??? + question "16. This copy of the macOS XXXX installation application is damaged and can't be used to install macOS."
   ![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16494864391246.webp)
    The reason is that the current time is too new, and the system we are installing is no longer maintained. Just change the time to 2015. For detailed operations, refer to an online article: [This copy of the macOS Mojave installation application is damaged and can't be used to install Mac OS](https://zhuanlan.zhihu.com/p/88597219)

??? + question "17. The laptop's Type - C has no video output."
    If you confirm that your Type - C is using the integrated graphics, then it's mostly related to the model. If it's a 16 - inch laptop model, change it to a 13 - inch one. If the integrated graphics ID is correct, there will mostly be a Type - C output signal.

??? + question "18. Copying EFI prompts that there is not enough free space on EFI."
    It's more of a problem with the USB flash drive. Remember to clear the recycle bin in macOS, and in Windows, you can manually delete the.Trashes junk files:

   ![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16495143059696.webp)

    Or in macOS, after mounting the EFI partition, use the command line to manually delete junk files:
    ```shell
    cd /Volumes/EFI && rm -rf.Trashes 
    ```

??? + question "19. After the installation code finishes running, the pictures of Magic Trackpad and Magic Mouse appear."
    There are two possibilities:

    1. USB is not customized. It is recommended to refer to the [USB customization tutorial](/6 - Practical Postures/6 - 1.html#reloaded) to customize again.
    2. Keyboard and mouse drivers are missing. Just install `VoodooPS2Controller.kext`.

??? + question "20. Lenovo ThinkPad X13 20T3 with 10th - generation U actually has a pretty perfect Hackintosh, and sleep is also great."
    Adjust the hibernation strategy to Linux in the BIOS to enable S3 sleep. Self - tested with normal power consumption overnight. This is specially recorded to give some experience to future generations.

??? + question "21. The I2C touchpad in the default polling mode doesn't work."
    Here, take the DELL08BC touchpad of Dell Latitude 3400 i5 - 8265U as an example. The default IRQ is 0x00000033(51) which is greater than 2F, but the default XOSI polling mode doesn't work. Actually, those with i2cAddress 0x2c are all rather troublesome as they lack SSCN. We can solve the problem by adding the following SSCN SSDT:

    ```c
    DefinitionBlock ("", "SSDT", 2, "LENOVO", "ICL     ", 0x20170001)
    {
        External (_SB_.PCI0.I2C0, DeviceObj)
        External (FMD1, IntObj)
        External (FMH1, IntObj)
        External (FML1, IntObj)
        External (SSD1, IntObj)
        External (SSH1, IntObj)
        External (SSL1, IntObj)
        External (TPDM, IntObj)

        Method (PKG3, 3, Serialized)
        {
            Name (PKG, Package (0x03)
            {
                Zero, 
                Zero, 
                Zero
            })
            PKG [Zero] = Arg0
            PKG [One] = Arg1
            PKG [0x02] = Arg2
            Return (PKG) /* \PKG3.PKG_ */
        }

        Scope (_SB.PCI0.I2C0)
        {
            Method (SSCN, 0, NotSerialized)
            {
                Return (PKG3 (SSH1, SSL1, SSD1))
            }

            Method (FMCN, 0, NotSerialized)
            {
                Return (PKG3 (FMH1, FML1, FMD1))
            }
        }
    }
    ```
    The following is the comparison before and after adding the SSCN SSDT. It can be seen that the one on the left (at the beginning) is indeed an incomplete I2C:

   ![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16513709163816.webp)

??? + question "22. The installation code finishes running, but during the subsequent installation, it prompts 'An error occurred while preparing for the software update'."
    This situation has been seen on some Dell laptops. Check the 「Enable Custom Mode」 option in the BIOS to solve this problem.

??? + question "23. After USB customization is completed, but USB3.X still doesn't work properly."
    This problem is common on 400 - series motherboards. In this case, install a [XHCI - unsupported.kext](https://sqlsec.lanzoub.com/i8CUI046dufe).

??? + question "24. When the ASUS motherboard boots, it prompts 'The system has POSTed in safe mode'."
    This problem is common on ASUS motherboards. Check the 「DisableRtcChecksum」 option in the Kernel of the OC configuration file.