## Official Theme
This is the theme provided by OpenCore officially:
[https://github.com/acidanthera/OcBinaryData/](https://github.com/acidanthera/OcBinaryData/)
You can also make your own icons with the same name for replacement, or download theme packages made by others for replacement. I have also posted some theme download websites before. See the "Drivers Explanation" section for details.
## Settings
- PickerMode -> External
- PickerAttributes -> 129(0x81)
## Drivers
- OpenCanopy.efi
## Required Icons
If these icons are missing, the theme will not take effect.

- **BtnFocus** - Focus of other (shutdown and restart) buttons (`BtnFocus.icns`)
- **Cursor** - Mouse cursor (`Cursor.icns`, the same below.)
- **Dot** - The hidden character dot for password input
- **Enter** - The input symbol for password input
- **Left** - The additional selector entry on the left
- **Lock** - The password lock
- **Password** - The password input text area
- **Restart** - Additional button: restart
- **Right** - The additional selector entry on the right
- **Selected** - The background of the selected boot entry
- **Selector** - Displays the selected entry
- **SetDefault** - Displays the selected entry in the "Set Default" mode
- **ShutDown** - Additional button: shutdown
- **ExtHardDrive** - Bootable operating system (system on an external drive, such as a USB flash drive)
- **HardDrive** - Bootable operating system (local hard disk)

## Automatically Detected Boot Entries
OpenCore will automatically detect whether the system is Windows or macOS to match the boot entry icons. macOS will automatically detect the system version, while Windows will not. As for whether it is Windows 11 or Windows 10... OpenCore can only detect that the system is Windows.
### Icons
### Windows
**HardDrive.icns** is the basic boot entry icon. If you don't customize an icon for the boot entry, all systems will display this icon.
- If you want to customize an icon for Windows, first design a Windows icon by yourself and name it **Windows.icns**. When OpenCore starts, it will automatically match this icon for it.
- The same applies to macOS. You need an **Apple.icns** icon, otherwise it will fall back to **HardDrive.icns**.

So the question is, when there are multiple Windows systems and multiple macOS systems on the same machine (a large hard disk allows you to be willful), how can different Windows or macOS systems display different icons?
- At this time, a file `.contentFlavour` is needed. You can create a txt text file with Notepad under the Windows system. When you want to make a separate boot entry icon for Windows 11, fill the content of this text file like this **Windows11:Windows**, which means you also need to design an icon file named `Windows11.icns`. After filling in the content of the text file, save and exit, and then rename the text file to `.contentFlavour`. The.txt suffix of the text file should also be deleted. The complete name of the text file is renamed to `.contentFlavour`, and then copy this file to the directory where the Windows boot file is located, usually `\EFI1\Microsoft\Boot\`.
- Similarly, if you have a Windows 10 system, change the content of the above - mentioned text file to **Windows10:Windows**, and you also need to prepare a `Windows10.icns` icon file.
- If the above - mentioned icon files are missing, they will fallback step by step. If `Windows10.icns` is missing, it will fallback to `Windows.icns`. If `Windows.icns` is also missing, it will finally fallback to `HardDrive.icns`.
### macOS
The macOS boot entry is not so troublesome and doesn't need the `.contentFlavour` file because OpenCore will automatically detect the macOS system version. You only need to do something with the icon files.
- `Apple12.icns` is automatically matched as the icon of Monterey.
- `Apple11.icns` is automatically matched as the icon of Big Sur.
- `Apple10_15.icns` is automatically matched as the icon of Catalina.
- `Apple10_14:Apple` is automatically matched as the icon of Mojave.
- `Apple10_13:Apple` is automatically matched as the icon of High Sierra.

And so on for others.
## Boot Entry Titles
### Windows
The way to customize the boot entry title is similar to that of customizing the icon, except that the txt file should be renamed to `.contentDetails`, and the content of this text file is the title content you want to customize. For example, if you want to name it Windows 11, the content of the text file is **Windows11**. This file is also placed in the directory where the boot file is located, `\EFI1\Microsoft\Boot\`.
### macOS
- If you want to name the boot entry title of macOS as Monterey, the content of the text file is `Monterey`, and copy the file to `/System/Volumes/Preboot/UUID - number/System/Library/CoreServices`. Everyone's UUID is different. You only need to go to `/System/Volumes/Preboot/` in the Finder, and then click the next directory with the mouse.
- This step should be carried out in the macOS environment. `.contentDetails` is a hidden file under Windows. Enter the following command in the terminal to display hidden files.

```shell
defaults write com.apple.finder AppleShowAllFiles -boolean true;killall Finder
```
Enter the following command in the terminal to hide hidden files.

```shell
defaults write com.apple.finder AppleShowAllFiles -boolean false;killall Finder
```
- This work requires disabling SIP. See the last part of the Misc section of the OC configuration for how to disable it.
## Manually Added Boot Entries
This is relatively simple. You don't need the two above - mentioned files. Just modify the configuration in the `config.plist` file.
There is an item `Flavour` in the `Misc -> Boot -> Entries` boot entry. The way to fill in the content of this item is the same as the way to fill in the custom boot entry icon for Windows detected automatically above. For example, if you want to customize an icon for Windows 11, fill it in like this `Windows11:Windows`.