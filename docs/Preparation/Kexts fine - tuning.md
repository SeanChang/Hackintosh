Kexts also have order requirements, and some Kexts may conflict and cause the system to be unable to boot. This part is also rather complicated. Since the developers of Kexts are different, I can only try my best to describe it. If the documentation description is not clear, you can watch the corresponding tutorial videos on Bilibili.

## Automatically Adjust the Order of Kexts
When ProperTree performs the operation of **Cmd/Ctrl + Shift + R** or uses the graphical menu to select 「Clean Snapshot」, it will automatically adjust the loading order of Kexts and disable the conflicting Kexts.

Actually, OpenCore Configurator also has this function. Click 「Check Kexts」 at the bottom:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16320620884857.webp) 

## Manually Adjust the Order of Kexts
The order automatically adjusted by the software is not necessarily the most accurate because it doesn't know which messy Kexts you have used. So sometimes we need to manually adjust the loading order of Kexts.

### OpenCore Configurator
The operation of OpenCore Configurator is relatively simple. Select the Kext you want to operate, and then directly drag it up and down to adjust the order:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16320622615904.webp) 

### OCAuxiliaryTools
Although OCAuxiliaryTools can't be directly dragged, you can use the 「up」 and 「down」 arrow icons at the bottom to adjust the loading order of Kexts:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16320623284819.webp)  

## Adjustment Details of Common Kexts

### Loading Order of Essential Kexts
1. Lilu.kext
2. VirtualSMC.kext
3. WhateverGreen.kext
4. SMCBatteryManager.kext (not needed for desktops)
5. SMCLightSensor.kext        (not needed for desktops)
6. SMCProcessor.kext 
7. SMCSuperIO.kext
8. AppleALC.kext

### Loading Order of BCM Broadcom Wireless and Bluetooth
1. AirportBrcmFixup.kext
2. BrcmBluetoothInjector.kext
3. BrcmFirmwareData.kext
4. BrcmPatchRAM3.kext

### Detailed Adjustment of BCM Broadcom Wireless
Directly loading AirportBrcmFixup.kext will actually load 3 Kexts:

1. AirportBrcmFixup.kext
2. AirportBrcmFixup.kext/Contents/PlugIns/AirPortBrcmNIC_Injector.kext
3. AirportBrcmFixup.kext/Contents/PlugIns/AirPortBrcm4360_Injector.kext

Among them, when installing Big Sur and later versions, AirPortBrcm4360_Injector.kext will have problems by default, so we need to set the maximum kernel to 19.9.9, which may reduce some abnormal errors:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16320628447557.webp)  



In addition, for network cards with the BCM94352Z chip, it is often necessary to add `brcmfx - driver = 2` in the boot item to solve some sleep or other mysterious problems:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16320629319077.webp)  

### Loading Order of Intel Wireless and Bluetooth
1. AirportItlwm.kext
2. IntelBluetoothInjector.kext
3. IntelBluetoothFirmware.kext

### Loading Order of Laptop PS2 Keyboard, Mouse, and Touchpad
1. VoodooPS2Controller.kext
2. VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext
3. VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Mouse.kext
4. VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Trackpad.kext
5. VoodooPS2Controller.kext/Contents/PlugIns/VoodooInput.kext
6. BrightnessKeys.kext (function brightness adjustment key driver, not always necessary)

### Loading Order of Laptop I2C and PS2 Cooperative Drive Touchpad
1. VoodooI2C.kext/Contents/PlugIns/VoodooI2CServices.kext
2. VoodooI2C.kext/Contents/PlugIns/VoodooGPIO.kext
3. VoodooI2C.kext
4. VoodooI2CHID.kext
5. VoodooPS2Controller.kext
6. VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext
7. VoodooI2C.kext/Contents/PlugIns/VoodooInput.kext
8. BrightnessKeys.kext  (function brightness adjustment key driver, not always necessary)

!!! tip "A small tip: Since both IC2 and PS2 have VoodooInput.kext, if the VoodooInput.kext of PS2 is not deleted or disabled, it will cause a kernel conflict during startup, resulting in system freeze and inability to boot."



### Loading Order of Laptop PS2Smart Keyboard Driver
1. ApplePS2SmartTouchPad.kext/Contents/PlugIns/ApplePS2Controller.kext
2. ApplePS2SmartTouchPad.kext/Contents/PlugIns/ApplePS2Keyboard.kext
3. ApplePS2SmartTouchPad.kext

### Detailed Adjustment of Laptop VoodooRMI.kext and VoodooSMBus.kext
First, make sure that the VoodooPS2 configuration is as follows:

- Enable
  - VoodooPS2Controller.kext
  - VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Mouse.kext
  - VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext
  - VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Trackpad.kext
- Disable
  - VoodooPS2Controller.kext/Contents/PlugIns/VoodooInput.kext

Then **enable** and load the basic RMI configuration:

- VoodooRMI.kext
- VoodooRMI.kext/Contents/PlugIns/VoodooInput.kext

If you have an SMBus touchpad, you also need to load:

- VoodooSMBus.kext
- VoodooRMI.kext/Contents/PlugIns/RMISMBus.kext

If you have an I2C touchpad, you also need to load:

- VoodooI2C.kext
- VoodooRMI.kext/Contents/PlugIns/RMII2C.kext