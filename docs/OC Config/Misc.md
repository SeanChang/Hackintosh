Misc miscellaneous part. This part is basically universal, so I like this chapter very much. In this way, there is no need to consider various models, and it is more comfortable to write.

## Boot
The following are my usage habits:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321530884272.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321530884272.webp) 

- **Picker Mode**: Select **External** for this, which means enabling an external theme (provided that you have copied the corresponding theme files).
- PickerVariant: You can manually select the theme you want to load here.
- PollAppleHotKeys:
    - It needs to be used in combination with the Quirk `KeySupport = Yes`.
    - This is my personal habit, which means simulating Apple hotkeys.
    - In this way, there is no need to add the `-v` parameter in the boot entry. You can directly use `Commad/Ctrl + V` on the system selection interface, which is very simple and convenient.

Shortcut key combinations:

- `Cmd + V`: Enable `-v` to display boot codes.
- `Cmd + Opt + P + R`: Reset NVRAM.
- `Cmd + R`: Boot into the recovery partition.
- `Cmd + S`: Boot into single - user mode.
- `Option / ALT`: When `ShowPicker` is set to `NO`, display the boot item selection interface. When `ALT` is unavailable, the `ESC` key can be used instead.
- `Cmd + C + Minus`: Turn off motherboard compatibility check, which is equivalent to adding the boot identifier `-no_compat_check`.
- `Shift`: Safe mode.

## Debug
This is mainly used for Debug debugging. Actually, the default settings can also be used. The following are the settings recommended by the OC official:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321533514537.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321533514537.webp) 

Among them, Target means:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321533997222.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321533997222.webp) 

The boot log will be recorded in the root directory of the EFI partition each time, which is convenient for debugging. When it is stable in the later stage, you can uncheck "Record to File", that is, the value of Target is `3`.

## Security
The following are also my usage habits:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321535333733.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321535333733.webp) 

- **Scan Policy** can be changed to 0. Actually, the selectable values are as follows:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321536046088.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321536046088.webp) 

Sometimes, when installing the system, there is a situation where OC cannot recognize the Windows boot entry. Checking "Allow scanning of EFI system partition file system" has solved this problem. Theoretically, changing Scan Policy to 0 can also solve this kind of problem.

- **Valut**: Optional.
If you make a mistake, theoretically, the computer will not be able to boot directly, so it is basically a required option.

- **AllowToggle**: Checked.
This is also my habit. After checking this, each time you turn SIP on or off, you only need to switch it on the system selection interface, which is very convenient:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321538984521.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321538984521.webp)