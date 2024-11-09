## Hardware

In the previous content, you have more or less understood the driver situation of the hardware. In this part, I will share the methods and techniques for determining your hardware model.

## Using Windows to View Hardware

### CPU Model

You can use the Device Manager or the built - in Task Manager in Windows, or use third - party software such as CPU - Z, AIDA64, and LuDaShi.

### GPU Model

You can use the Device Manager or the built - in Task Manager in Windows, or use third - party software such as GPU - Z, AIDA64, and LuDaShi.

### Chipset Model

Device Manager:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317473388157.webp) 

Or use third - party software like AIDA64:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317470304733.webp) 

### Sound Card Model

It is recommended to use third - party software such as AIDA64 and LuDaShi:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317473047255.webp) 

### Network Card Model

You can use the Device Manager or AIDA64. It is still recommended to use third - party software to view:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317472872822.webp) 

### Hard Disk Model

For hard disks, detailed information usually can't be seen through software. You can only confirm it with the help of Google:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317472676295.webp) 

### Keyboard, Touchpad, Touchscreen Connection Types

Unfortunately, AIDA64 doesn't provide any useful information for these types of devices. So we recommend using the Device Manager for this purpose.

- You can find these devices in the following locations:
  - `Human Interface Devices` (HID) 
  - `Keyboards`
  - `Mice and other Pointer Devices`
- To view the exact connection type of the device, select the pointer device and then enter `View -> Device by Connection`. This will more intuitively show whether it is connected via PS2, I2C, SMBus, USB, etc.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317475714983.webp) 

Depending on the device, it may be displayed under multiple names and connections. Mainly focus on the following points:

#### SMBus

Generally, the driver support for this kind of touchpad is relatively friendly, and the gestures are perfect. In the Device Manager, it will be shown as a straight - line PCI device, such as `Synaptics SMBus Driver` or `ELAN SMBus Driver`.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317476815770.webp) 

#### USB

These will be shown as a `PS2 Compliant Trackpad`, and in USB when we switch our connection view to `Device by Connection`.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317477867708.webp) 

#### I2C

These are almost always shown as Microsoft HID devices, but they can also be shown as other touchpads. However, they will always be shown under I2C.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16317478271178.webp) 

## Using Linux to View Hardware

To find hardware using Linux, we will use the following commands:

- `pciutils`
- `dmidecode`

Most Linux distributions have already installed these tools. If not, you may find them in the package manager of the distribution.

```bash
# CPU model
grep -i "model name" /proc/cpuinfo

# Graphics card model
lspci | grep -i --color "vga\|3d\|2d"

# Chipset model
dmidecode -t baseboard

# Sound card model
aplay -l

# Network card model
lspci | grep -i network
lshw -class networ

# Hard disk model
lshw -class disk -class storage

# Keyboard, touchpad, touchscreen connection types
dmesg | grep -i input
```