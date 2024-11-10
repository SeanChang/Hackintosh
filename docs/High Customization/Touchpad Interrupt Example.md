## Preface

When you see this part of the content, it means that you have already delved quite deeply into Hackintosh, and the touchpad working in polling mode no longer satisfies you. You won't be satisfied until you get a GPIO interrupt. Actually, this content is taken from this article on my blog: [Research on Touchpad Drivers for Dual - screen Laptops under macOS](https://www.sqlsec.com/2021/12/screenpad.html) 

Since the article is rather long, I've extracted the key parts and put them in this tutorial. Interested friends can also go to the original article to read it. Now, let's directly start the part of the touchpad interrupt tutorial.

## Hardware Information

Before handling the interrupt, first collect the basic information of your touchpad. First, briefly organize the touchpad information under Windows. A standard touchpad that complies with the Microsoft I2C protocol is used:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399193918328.webp) 

Under macOS, [IORegistryExplorer](https://github.com/vulgo/IORegistryExplorer) can also be used to identify that the model of the touchpad is: **GDX1515**

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399186829443.webp) 

The above potentially useful information is organized as follows:

- **The name of this 2 - in - 1 touchpad under Windows**: ScreenPad
- **Touchpad model**: GDX1515
- **Touchpad screen model**: ScreenXpert
- **IRQ**: `0x0000006D (109)`
- **APIC Pin**: `6d（109）`
- **Device instance path**: `ACPI\GDX1515\1`
- **Hardware Id**:
  - `ACPI\VEN_GDX&DEV_1515`
  - `ACPI\GDX1515`
  - `*GDX1515`
- **BIOS device name**: `\_SB.PCI0.I2C1.ETPD`
- **Location path**:
  - `ACPI(_SB)#ACPI(PCI0)#(I2C1)#ACPI(ETPD)`
  - `ACPI(_SB)#ACPI(USBX)#(I2C1)#ACPI(ETPD)`

## Working Modes of the Touchpad

The more perfect situation is to make the touchpad work in interrupt mode (interrupts). For polling and interrupt, you can refer to the following concepts:

- APIC Interrupt
  - The interrupt mode used by macOS, with perfect functionality, and only a very few devices support it.
  - It is only supported when the APIC Pin value is less than `2F (47)`.
- GPIO Interrupt
  - The interrupt mode mostly used in the Windows system.
  - It is second only to the APIC interrupt and is relatively efficient, but you need to modify and customize the SSDT yourself.
- Polling
  - A relatively inefficient mode.
  - But it has relatively good compatibility and is applicable to most touchpads.
  - It is prone to problems such as pointer drift and other unresponsive BUGs.

So, in what mode is our current touchpad working? From the previous information collection, we know that the APIC Pin value of the touchpad is:

- **IRQ**: `0x0000006D (109)`
- **APIC Pin**: `6d（109）`

## Judging the Working Mode of the Touchpad

So, how to judge the current working mode of the touchpad? There are currently three methods as follows:

### Viewing the dmesg Log

Under normal circumstances, using the dmesg command, you can directly search for the working state of interrupt or polling based on the keywords of the touchpad model:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399882815239.webp) 

However, Hackintosh is never smooth sailing. As you can see, the log result viewed by dmesg is empty. This is because the macOS 12.0 system restricts the content viewed by dmesg. Here, we need to manually load kexts to view the log.

#### Turning off SIP

Many OC configurations have SIP enabled by default. Since we want to load our own Kexts, we need to manually switch the SIP status on the interface where we choose the system at boot:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1639988468424.jpeg) 

If your OC doesn't have this option, it's because this configuration switch isn't checked. For specific details, you can refer to this part of my article: [My Hackintosh Installation Tutorial: Teach You to Configure OpenCore Step by Step](https://apple.sqlsec.com/4 - OC配置/4 - 5.html) 

#### Disabling Kexts in OC

The kexts injected through OpenCore can't be used to view the dmesg log normally. So, we need to manually disable the relevant kexts:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399886935508.webp) 

#### Manually Loading Kexts

Copy `VoodooI2C.kext` and `VoodooI2CHID.kext` to the desktop, and then execute the following commands:

```bash
# Modify the owner
sudo chown -R root:wheel VoodooI2C*

# Modify the permissions
sudo chmod -R 755 VoodooI2C*

# Load kexts
sudo kextload -v VoodooI2C*
```

Of course, the first time you load kexts, you need to agree to the permission in the "Security & Privacy" setting, and then restart the computer to successfully load them: 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399905575583.webp) 

After the above operations, the final viewing effect is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399893773012.webp) 

It can be seen that from the dmesg log, this GDX1515 touchpad is working in polling mode.

> For the details of log viewing, a separate subsection will be opened below to introduce them in detail.

### Using IORegistryExplore

The above method may be a bit cumbersome. Actually, using [IORegistryExplorer](https://github.com/vulgo/IORegistryExplorer) can also directly show the current working state of the touchpad. Because under the device manager in Windows, the location path of my touchpad is:

- `ACPI(_SB)#ACPI(PCI0)#(I2C1)#ACPI(ETPD)`
- `ACPI(_SB)#ACPI(USBX)#(I2C1)#ACPI(ETPD)`

So, searching for `ETPD` can show the detailed information of our touchpad. Generally, two results will be searched out. The following is the first result:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1639989561260.webp) 

However, the first result has no reference value. We usually focus on the second result searched out:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1640052163711.webp) 

The above picture is a typical situation where it is not working in interrupt mode.

At this time, some netizens will surely ask, if it is in GPIO interrupt mode, what should it look like here?

2333 This is a good question. I consulted [Bat.bat](https://github.com/williambj1) guru. The following are the original words of the guru:

------
When the code reaches GPIO, there will be a set property to write IRQ and Pin to ioreg. So, when using IORegistryExplorer to view, just pay attention to whether these two new properties exist.
------

Speaking of this, some netizens may still not be very clear. Below, I will post a screenshot of IORegistryExplorer in GPIO interrupt mode, and everyone should understand:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399899139722.jpeg) 

## Customizing and Patching DSDT

### Extracting DSDT

Since making the touchpad definitely requires customizing the SSDT, extracting the original DSDT of the motherboard is essential.

There are many methods to extract DSDT. You can use Clover. Under Windows, you can use AIDA64. Under macOS, you can use [DPCIManager](https://github.com/MuntashirAkon/DPCIManager). Just open it and click "Extract DSDT" in the upper - left corner:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399912929359.webp) 

### DSDT Troubleshooting

You can directly load DSDT in ACPI through OC, or you can separately extract the relevant code of the touchpad in DSDT and save it as an SSDT file. Both methods are fine. Here, I directly modify it in DSDT (simple and crude).

For DSDT troubleshooting, the essential [MaciASL](https://github.com/acidanthera/MaciASL) software is used here. Just click "Compile" to compile. If I'm not wrong, there will definitely be a bunch of errors:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399924216669.webp) 

This is because the ACPI specifications of the DSDT of each motherboard are not unified. So, we have to modify the errors according to our own programming experience. It's not easy to describe this in an article because each motherboard is different. My errors here are relatively easy to understand. I just deleted these `Zero` codes directly. 23333 (Specific troubleshooting has to be done according to the corresponding error situation)

Finally, it's successfully 0 errors:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399927496136.jpeg) 

If DSDT has no problem, it can be directly loaded using OC:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16400533116218.webp) 

## Finding the Touchpad Code

Since from the previous information collection, we know that the path of the touchpad is `ETPD`, we can directly search for ETPD to find the touchpad code.

It is found that the path in the lower - left corner also meets the situation of our previous information collection (`ACPI(_SB)#ACPI(PCI0)#(I2C1)#ACPI(ETPD)`):

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1639993158804.jpeg) 

The touchpad code part is pasted below:

```c
Scope (_SB.PCI0.I2C1)
    {
        Device (ETPD)
        {
            Name (_ADR, One)  // _ADR: Address
            Name (ETPH, Package (0x01)
            {
                "ASUE1407"
            })
            Name (FTPH, Package (0x09)
            {
                "FTE1001", 
                "FTE1200", 
                "FTE1200", 
                "FTE1300", 
                "FTE1300", 
                "FTE1201", 
                "FTE1200", 
                "FTE1200", 
                "FTE1200"
            })
            Name (GTPH, Package (0x05)
            {
                "GDX1505", 
                "GDX1300", 
                "GDX1200", 
                "GDX1301", 
                "GDX1515"
            })
            Method (_HID, 0, NotSerialized)  // _HID: Hardware ID
            {
                If (And (TPDI, 0x04))
                {
                    Return (DerefOf (Index (ETPH, TPHI)))
                }

                If (And (TPDI, 0x10))
                {
                    Return (DerefOf (Index (FTPH, TPHI)))
                }

                If (And (TPDI, 0x40))
                {
                    Return (DerefOf (Index (GTPH, TPHI)))
                }

                Return ("ELAN1010")
            }
          
          	Name (_CID, "PNP0C50")  // _CID: Compatible ID
            Name (_UID, One)  // _UID: Unique ID
            Name (_S0W, 0x03)  // _S0W: S0 Device Wake State
            Method (_DSM, 4, NotSerialized)  // _DSM: Device - Specific Method
            {
                If (LEqual (Arg0, ToUUID ("3cdff6f7-4267-4555-ad05-b30a3d8938de") /* HID I2C Device */))
                {
                    If (LEqual (Arg2, Zero))
                    {
                        If (LEqual (Arg1, One))
                        {
                            Return (Buffer (One)
                            {
                                 0x03                                           
                            })
                        }
                        Else
                        {
                            Return (Buffer (One)
                            {
                                 0x00                                           
                            })
                        }
                    }
                  
                  If (LEqual (Arg2, One))
                    {
                        Return (One)
                        }
                    }
                    Else
                    {
                        Return (Buffer (One)
                        {
                             0x00                                           
                        })
                    }
                }
          Method (_STA, 0, NotSerialized)  // _STA: Status
          {
                      If (LOr (LNotEqual (TPIF, One), LAnd (DSYN, One)))
                      {
                          Return (Zero)
                      }

                      Return (0x0F)
               }
          		
          Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
          {
                Name (SBFI, ResourceTemplate ()
                {
                    I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
                        AddressingMode7Bit, "\\_SB.PCI0.I2C1",
                        0x00, ResourceConsumer,, Exclusive,
                        )
                    Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive,,, )
                    {
                        0x0000006D,
                    }
                })
                Return (SBFI)
            }
        }
    }
}
```

## GPIO Pinning Fixing

Here, I only introduce how to customize the GPIO interrupt. Polling and other modes are not within the scope of this article.

### Finding the Key Touchpad Code

According to the feature of the `_CRS` method, we can easily find a code snippet similar to the following in the touchpad code:

```c
Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
{
  Name (SBFI, ResourceTemplate ()
        {
          I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
                          AddressingMode7Bit, "\\_SB.PCI0.I2C1",
                          0x00, ResourceConsumer,, Exclusive,
                         )
            Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive,,, )
          {
            0x0000006D,
          }
        })
    Return (SBFI)
}
```

### Rename SBFI

When VoodooI2C calls the _CRS method of the touch device in the DSDT in GPIO interrupt mode, it always uses the `SBFG` parameter instead of the `SBFI` parameter. So the `SBFI` variable in our current touchpad code doesn't meet the requirements. We first rename `SBFI` to `SBFB`. As for the `SBFG` variable, we will add it separately later (if it doesn't exist in your DSDT).

```c
Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
{
  Name (SBFB, ResourceTemplate ()
        {
          I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
                          AddressingMode7Bit, "\\_SB.PCI0.I2C1",
                          0x00, ResourceConsumer,, Exclusive,
                         )
  //         Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive,,, )
          
  //        {
  //          0x0000006D,
  //        }
        })
    Return (SBFB)
}
```
After renaming, remove the following content in the _CRS method (the part commented in the above code):

```c
Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive,,, )
{
  0x0000006D,
}
```

### Look for GPIO Pin

Look for a code snippet similar to the following in the touchpad code:

```c
Name (SBFG, ResourceTemplate ()
{
  	GpioInt (Level, ActiveLow, ExclusiveAndWake, PullDefault, 0x0000,
  		"\\_SB.PCI0.GPI0", 0x00, ResourceConsumer,,
	)
  {   // Pin list
      0x0000
  }
})
```
If you find it, congratulations. Your device may not need to calculate the GPIO Pin value. It doesn't matter if you don't find it. We can add it manually. See the details below.

### Add GPIO Template

Obviously, I can't find the GPIO - related code in my touchpad. So we need to copy the above code snippet and add it under the _CRS method:

```c
Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
{
  Name (SBFB, ResourceTemplate ()
        {
          I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
                          AddressingMode7Bit, "\\_SB.PCI0.I2C1",
                          0x00, ResourceConsumer,, Exclusive,
                         )
        })
    // The following is the newly added GPIO template code snippet
    Name (SBFG, ResourceTemplate ()
          {
            GpioInt (Level, ActiveLow, ExclusiveAndWake, PullDefault, 0x0000,
                     "\\_SB.PCI0.GPI0", 0x00, ResourceConsumer,,
                    )
            {   // Pin list
              0x0000   // We will calculate this value later
            }
          })
    Return (SBFB)
}
```

### Modify the _CRS Return Value

Since we have also introduced the SBFG variable in the _CRS method, we need to change the default `Return (SBFB)` return value to:

```c
Return (ConcatenateResTemplate (SBFB, SBFG))
```

> Original words of the big shot [Bat.bat](https://github.com/williambj1): For devices with gpioint, there is no need to calculate the pin. For those without it, adding it usually won't succeed. Calculating the pin is actually the last - ditch solution.

### Calculate the GPIO Pin Value

In the template we added above, the GPIO Pin value is `0x0000`. Generally, this won't work and can only serve as a placeholder. So we need to calculate a correct GPIO value. This step is crucial. Different CPUs have different calculation formulas. The following formula is also provided by [Bat.bat](https://github.com/williambj1).

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16400555425472.webp) 

- **Skylake** (intel 6th - generation CPU)

```c
If APICPIN > 47 And APICPIN <= 79 Then     
    GPIOPIN = APICPIN - 24   
    GPIOPIN2 = APICPIN + 72  
ElseIf APICPIN > 79 And APICPIN <= 119 Then
    GPIOPIN = APICPIN - 24
End If
```

- **CoffeeLake - H** (intel 8th - generation high - voltage CPU)

```c
If APICPIN > 47 And APICPIN <= 71 Then   
    GPIOPIN = APICPIN - 16   
    GPIOPIN2 = APICPIN + 240 
    If APICPIN > 47 And APICPIN <= 59 Then GPIOPIN3 = APICPIN + 304  
ElseIf APICPIN > 71 And APICPIN <= 95 Then 
    GPIOPIN = APICPIN - 8    
    GPIOPIN3 = APICPIN + 152
    GPIOPIN2 = APICPIN + 120 
ElseIf APICPIN > 95 And APICPIN <= 119 Then 
    GPIOPIN = APICPIN        
If APICPIN > 108 And APICPIN <= 115 Then 
  	GPIOPIN2 = APICPIN + 20 
End If
```

- **CoffeeLake - LF and Whiskylake** (intel 8th - generation low - voltage CPU and Whiskylake - architecture CPU)

```c
If APICPIN > 47 And APICPIN <= 71 Then      
    GPIOPIN = APICPIN - 16   
    GPIOPIN2 = APICPIN + 80  
ElseIf APICPIN > 71 And APICPIN <= 95 Then  
    GPIOPIN2 = APICPIN + 184 
    GPIOPIN = APICPIN + 88   
ElseIf APICPIN > 95 And APICPIN <= 119 Then 
    GPIOPIN = APICPIN        
ElseIf APICPIN > 108 And APICPIN <= 115 Then 
  	GPIOPIN2 = APICPIN - 44 
End If
```

The CPU of my laptop is `i7 - 10510U`, which is a low - voltage CPU of Comet Lake. We will find that there is no formula for the Comet Lake series above. However, [Bat.bat](https://github.com/williambj1) said that the 10th - generation is a clone of the 8th - generation. So we can use the formula of CoffeeLake - LF and Whiskylake. By substituting the formula, we can calculate the decimal value of our GPIO Pin:

The hexadecimal value of our APICPIN is 6d, and its decimal value is 109, which satisfies the following condition of the formula:

```c
ElseIf APICPIN > 95 And APICPIN <= 119 Then 
    GPIOPIN = APICPIN    // GPIOPIN = 109
```
That is, GPIOPIN = 109, and its hexadecimal value is `6d`.

At the same time, we will find that our APICPIN also satisfies the following condition:

```c
ElseIf APICPIN > 108 And APICPIN <= 115 Then 
  	GPIOPIN2 = APICPIN - 44  // GPIOPIN2 = 109 - 44 = 65
```
That is, GPIOPIN = 65, and its hexadecimal value is `41`.

We can see that we have calculated two values, `6d` and `41`, and we can try to verify them one by one.

> In very rare cases, the calculated GPIO Pin value doesn't work. In this case, you can try some common values: `0x17`, `0x17B`, `0x34` and `0x55`.

Finally, I tried the GPIO Pin value of my GDX1515 touchpad as `6d`:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16400562947066.webp) 

Here is a picture for a more intuitive view.

## Final Effect

Using [IORegistryExplorer](https://github.com/vulgo/IORegistryExplorer), we can see that our GDX1515 niche touchpad finally works in GPIO interrupt mode. We can see the `gpioPin` and `gpioIRQ` attribute values. It's perfect:

![img](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1640056552178.webp)

Let's also look at the dmesg log situation:

```ini
# Found the GDX1515 touchpad of the I2C protocol
[   44.313108]: VoodooI2CControllerDriver::pci8086,2e9 Found I2C device: GDX1515

# ETPD found a valid _CRS method
[   44.313178]: VoodooI2CDeviceNub::ETPD Found valid resources from _CRS method
[   44.313182]: VoodooI2CControllerDriver::pci8086,2e8 Got bus configuration values
[   44.313231]: VoodooI2CDeviceNub::ETPD Returned index 0x0 from _DSM or XDSM method is not supported
[   44.313235]: VoodooI2CDeviceNub::ETPD Could not retrieve resources from _DSM or XDSM method

# ETPD found a valid GPIO interrupt
[   44.313244]: VoodooI2CDeviceNub::ETPD Found valid GPIO interrupts
[   44.313344]: VoodooI2CControllerDriver::pci8086,2e8 Publishing device nubs

# ETPD got the GPIO controller
[   44.313347]: VoodooI2CDeviceNub::ETPD Got GPIO Controller! VoodooGPIOCannonLakeLP
[   44.816012]: VoodooI2CHIDDevice:0x100000738 start

# The reset of the GDX1515 device startup is completed
[   44.919729]: VoodooI2CHIDDevice::GDX1515 Device initiated reset accomplished
[   45.050029]: VoodooI2CHIDDevice:0x100000738 creating interfaces
[   45.051068]: VoodooI2CHIDDevice:0x100000738 Matching has vendor DeviceUsagePage : ff0c bundleIdentifier com.apple.AppleUserHIDDrivers ioclass AppleUserHIDEventService but transport and vendorID is missing
[   45.053582]: VoodooI2CPrecisionTouchpadHIDEventDriver:0x10000073d start
[   45.059693]: open by VoodooI2CPrecisionTouchpadHIDEventDriver 0x10000073d (0x0)

# GDX1515 enters Precision Touchpad Mode (PTP) mode, that is, high - precision touchpad mode
[   45.059739]: VoodooI2CPrecisionTouchpadHIDEventDriver::GDX1515 Putting device into Precision Touchpad Mode
```
We can also see from the log that it works perfectly in GPIO interrupt mode.

For comparison, the log situations when it doesn't work in GPIO interrupt mode are attached below:

- **Log in polling mode without custom DSDT**

```ini
[   48.517816]: VoodooI2CControllerDriver::pci8086,2e9 Found I2C device: GDX1515
[   48.517869]: VoodooI2CDeviceNub::ETPD Found valid resources from _CRS method
[   48.517913]: VoodooI2CDeviceNub::ETPD Returned index 0x0 from _DSM or XDSM method is not supported
[   48.517917]: VoodooI2CDeviceNub::ETPD Could not retrieve resources from _DSM or XDSM method

# ETPD warning, found an incompatible APIC pin value 6d, which is greater than 2f
[   48.517925]: VoodooI2CDeviceNub::ETPD Warning: Incompatible APIC interrupt pin (0x6d > 0x2f)

# ETPD can't find any APIC or GPIO interrupts. You may run in polling mode.
[   48.517931]: VoodooI2CDeviceNub::ETPD Warning: Could not find any APIC nor GPIO interrupts. Your chosen satellite will run in polling mode if implemented.
[   48.519529]: VoodooI2CHIDDevice:0x100000725 start

# GDX1515 can't get the interrupt event source and switches to polling mode
[   48.519539]: VoodooI2CHIDDevice::GDX1515 Warning: Could not get interrupt event source, using polling instead
[   48.722472]: VoodooI2CHIDDevice::GDX1515 Device initiated reset accomplished
[   48.854707]: VoodooI2CHIDDevice:0x100000725 creating interfaces
[   48.856351]: VoodooI2CHIDDevice:0x100000725 Matching has vendor DeviceUsagePage : ff0c bundleIdentifier com.apple.AppleUserHIDDrivers ioclass AppleUserHIDEventService but transport and vendorID is missing
[   48.860549]: VoodooI2CPrecisionTouchpadHIDEventDriver:0x10000072a start
[   48.866671]: open by VoodooI2CPrecisionTouchpadHIDEventDriver 0x10000072a (0x0)

# GDX1515 enters Precision Touchpad Mode (PTP) mode, that is, high - precision touchpad mode
[   48.866716]: VoodooI2CPrecisionTouchpadHIDEventDriver::GDX1515 Putting device into Precision Touchpad Mode
```

- **Log with custom DSDT but incorrect GPIO**

```ini
[   37.835221]: VoodooI2CControllerDriver::pci8086,2e9 Found I2C device: GDX1515
[   37.835266]: VoodooI2CDeviceNub::ETPD Found valid resources from _CRS method
[   37.835298]: VoodooI2CDeviceNub::ETPD Returned index 0x0 from _DSM or XDSM method is not supported
[   37.835306]: VoodooI2CDeviceNub::ETPD Could not retrieve resources from _DSM or XDSM method
[   37.835309]: KextLog: AuxKC bundle com.alexandred.VoodooI2CServices marked as loadable

# ETPD found valid GPIO interrupts.
[   37.835312]: VoodooI2CDeviceNub::ETPD Found valid GPIO interrupts
[   37.835408]: VoodooI2CDeviceNub::ETPD Got GPIO Controller! VoodooGPIOCannonLakeLP
[   38.337232]: VoodooI2CHIDDevice:0x100000761 start

# Failed to obtain the hardware pin 81 of the GPIO pin (decimal of the wrong GPIO Pin), failed to obtain the interrupt event source, and switched to polling instead.
[   38.337263]: VoodooGPIOCannonLakeLP::Failed getting hardware pin for GPIO pin 81VoodooI2CHIDDevice::GDX1515 Warning: Could not get interrupt event source, using polling instead
[   38.539715]: VoodooI2CHIDDevice::GDX1515 Device initiated reset accomplished
[   38.670485]: VoodooI2CHIDDevice:0x100000761 creating interfaces
[   38.672136]: VoodooI2CHIDDevice:0x100000761 Matching has vendor DeviceUsagePage : ff0c bundleIdentifier com.apple.AppleUserHIDDrivers ioclass AppleUserHIDEventService but transport and vendorID is missing
[   38.676469]: VoodooI2CPrecisionTouchpadHIDEventDriver:0x100000766 start
[   38.682612]: open by VoodooI2CPrecisionTouchpadHIDEventDriver 0x100000766 (0x0)

# GDX1515 enters Precision Touchpad Mode (PTP) mode, that is, high - precision touchpad mode.
[   38.682668]: VoodooI2CPrecisionTouchpadHIDEventDriver::GDX1515 Putting device into Precision Touchpad Mode
```