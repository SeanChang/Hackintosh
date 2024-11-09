Kernel - related option settings.

## Complete Quirks

Since the versions of each platform are different, before the detailed description, here is a complete Quirks description of OC 0.7.3, so that everyone generally has an idea when setting.

- **AppleCpuPmCfgLock**
    - If CFG - Lock in your BIOS has been turned off, this is not required.
- **AppleXcpmCfgLock**
    - If CFG - Lock in your BIOS has been turned off, this is not required.
- **AppleXcpmExtraMsrs**
    - Disable multiple critical MSR accesses for certain CPUs without native XCPM support.
    - It is mainly used on CPUs without native power management.
    - Generally, only these three types of CPUs, namely `Haswell - E`, `Broadwell - E`, and `Skylake - X`, are checked for use.
- **AppleXcpmForceBoost**
    - Force maximum performance in XCPM mode.
    - Force to increase the turbo boost. It is recommended to be used on professional equipment with long - term high load.
    - Some Xeon series processors will benefit from this option being enabled.
- **CustomSMBIOSGuid**
    - Perform GUID patching for the UpdatesBiosModeCustom custom mode.
    - Dell laptops usually need to have this checked.
    - If your laptop cannot display the serial number normally, you can also try checking this.
- **DisableIoMapper**
    - Disable IOMapper support in XNU (VT - d).
    - If VT - d is disabled in the BIOS, then there is no need to check this.
- **DisableLinkeditJettison**
    - This option allows Lilu.kext and possibly other kext to run at the optimal performance level in macOS Big Sur.
    - And there is no need to add keepsyms = 1 in the boot entry, which is equivalent to replacing it.
- **DisableRtcChecksum**
    - Disable writing the main checksum in AppleRTC.
- **ExtendBTFeatureFlags**
    - Set FeatureFlags to 0x0F to achieve all the functions of Bluetooth.
- **ForceSecureBootScheme**
    - Force the x86 scheme for IMG4 verification.
    - When using a SecureBootModel different from x86 legacy, this option is required on virtual machines.
- **IncreasePciBarSize**
    - Increase the 32 - bit PCI Bar size in IOPCIFamily from 1GBs to 4GBs.
    - The need for this option indicates a firmware configuration error or defect, so it is generally not used either.
- **LapicKernelPanic**
    - Disable kernel panics caused by AP core lapic interrupts.
    - For kernel crashes of HP laptops. If there is no crash, it is not recommended to check this.
- **LegacyCommpage**
    - Replace the default 64 - bit commpage bcopy implementation with an implementation that does not require SSE3.
    - This is very useful for traditional old platforms 10.4 - 10.6.
    - New platforms generally do not check this for use.
- **PanicNoKextDump**
    - Prevent the output of the Kext list when a kernel panic occurs, and provide a crash log for troubleshooting reference.
    - It is recommended to enable this when troubleshooting.
- **PowerTimeoutKernelPanic**
    - Fix kernel panics in macOS Catalina caused by device power state change time - outs.
    - Check this when encountering situations where the computer cannot wake up from sleep and can only be woken up after a restart.
    - Desktop computers generally do not check this.
- **ProvideCurrentCpuInfo**
    - Provide current CPU information to the kernel.
- **SetApfsTrimTimeout**
    - Versions before 10.14 do not need to be set.
    - Mainly for SATA SSDs, different delays are set according to different controllers.
- **ThirdPartyDrives**
    - Enable the TRIM instruction for SSDs, which may improve hibernation.
    - NVMe SSDs will be automatically loaded by macOS, so this is not required.
    - SATA SSDs can be enabled by executing `sudo trimforce enable` in the terminal, and the effect is the same.
- **XhciPortLimit**
    - Remove the 15 - port limit. If the USB ports are perfectly customized, this may not be checked.

## Intel Desktop Platform

### Yonah, Conroe, Penryn

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321478833493.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321478833493.webp) 

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321481054355.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321481054355.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321492912265.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321492912265.webp) 

### Lynnfield, Clarkdale, Sandy Bridge, Ivy Bridge

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321482138736.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321482138736.webp) 

CustomSMBIOSGuid: Consider enabling for Dell or VIAO machines.

LapicKernelPanic: Consider enabling for HP machines.

XhciPortLimit: If your computer does not have USB 3.0, do not enable it.

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321491215184.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321491215184.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321492413476.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321492413476.webp) 

### Haswell, Broadwell, Skylake, Kaby Lake, Coffee Lake, Comet Lake

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321494977418.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321494977418.webp) 

AppleCpuPmCfgLock: Required for systems 10.10 or older.

CustomSMBIOSGuid: Consider enabling for Dell or VIAO machines.

LapicKernelPanic: Consider enabling for HP machines.

XhciPortLimit: Remove the 15 - port limit. If the USB ports are perfectly customized, this may not be checked.

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321496494167.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321496494167.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321497103620.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321497103620.webp) 

## Intel High - end Desktop Platform

### Nehalem, Westmere, Sandy and Ivy Bridge - E, Haswell - E, Broadwell - E

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321502827693.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321502827693.webp) 

CustomSMBIOSGuid: Consider enabling for Dell or VIAO machines.

LapicKernelPanic: Consider enabling for HP machines.

XhciPortLimit: Remove the 15 - port limit. If your computer does not have USB 3.0, do not enable it, or if the USB ports are perfectly customized, this may not be checked.

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321503933269.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321503933269.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321504594789.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321504594789.webp) 

### Skylake - X/W and Cascade Lake - X/W

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321508912861.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321508912861.webp) 

CustomSMBIOSGuid: Consider enabling for Dell or VIAO machines.

LapicKernelPanic: Consider enabling for HP machines.

XhciPortLimit: Remove the 15 - port limit. If your computer does not have USB 3.0, do not enable it, or if the USB ports are perfectly customized, this may not be checked.

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321509912550.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321509912550.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321511219790.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321511219790.webp) 

## Intel Laptop Platform

### Clarksfield, Arrandale, Sandy Bridge, Ivy Bridge,

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321513228111.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321513228111.webp) 

CustomSMBIOSGuid: Consider enabling for Dell or VIAO machines.

LapicKernelPanic: Consider enabling for HP machines.

XhciPortLimit: Remove the 15 - port limit. If your computer does not have USB 3.0, do not enable it, or if the USB ports are perfectly customized, this may not be checked.

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321514193625.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321514193625.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321514754316.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321514754316.webp) 

### Haswell, Broadwell, Skylake, Kaby Lake, Coffee Lake, Whiskey Lake, Coffee Lake Plus, Comet Lake, Icelake

#### ProperTree

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321516364085.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321516364085.webp) 

AppleCpuPmCfgLock: Required for installing systems 10.10 or older.

CustomSMBIOSGuid: Consider enabling for Dell or VIAO machines.

LapicKernelPanic: Consider enabling for HP machines.

XhciPortLimit: Remove the 15 - port limit. If your computer does not have USB 3.0, do not enable it, or if the USB ports are perfectly customized, this may not be checked.

#### OpenCore Configurator

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321517134425.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321517134425.webp) 

#### OCAuxiliaryTools

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321517522639.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321517522639.webp) 

## AMD Desktop Platform

### Bulldozer(15h), Jaguar(16h), Ryzen, Threadripper(17h and 19h)

The AMD platform is a little special. You also need to check **DummyPowerManagement**. Other Quirks can be configured with reference to the following figure:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1632152312276.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1632152312276.webp) 

In addition, you also need to apply the [Path patch](https://github.com/AMD - OSX/AMD_Vanilla/blob/master/patches.plist). Since there are many attributes, it is recommended that you operate in text mode for higher efficiency:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321529335538.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321529335538.webp)