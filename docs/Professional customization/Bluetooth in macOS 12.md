## Broadcom Network Card with No - driver - required Feature

Just replace `BrcmBluetoothInjector.kext` with `BlueToolFixup.kext`.

The final drivers used are as follows:

- BlueToolFixup.kext
- BrcmFirmwareData.kext
- BrcmPatchRAM3.kext

## Intel Wireless Network Card

Just replace `IntelBluetoothInjector.kext` with `BlueToolFixup.kext`.

!!! quote "BlueToolFixup.kext can be downloaded from here: [https://github.com/acidanthera/BrcmPatchRAM/releases](https://github.com/acidanthera/BrcmPatchRAM/releases)"

The final drivers used are as follows:

- BlueToolFixup.kext
- IntelBluetoothFirmware.kext