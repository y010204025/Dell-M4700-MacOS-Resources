# Dell Precision M4700 MacOS
    
Releasing this to help others with Dell Precision M4700s.  
Please do not blindly copy. Understand and read the links provided, and realize that this may be out of date depending on when you read this. I would recommend reading [this guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/) and [this first contact page](https://internet-install.gitbook.io/macos-internet-install/) as well before proceeding. Anything Graphics related in this will *not* work if you have any of the Nvidia cards

## Specs

| CPU |  I7-3740QM  |
|---|---|
| RAM  | 4 x 4GB 1600Mhz DDR3  |
| GPU  | AMD Firepro Mobility M4000  |
| Screen  | 1080p 10 bit LVDS Panel  |
| Wifi  |  Broadcom BCM94352Z  |
| Trackpad  | Alphs Dual Point - V3 Rushmore  |

## Kexts
You can get more information about these by clicking on the links, which leads to their respective github repo.

* [Lilu](https://github.com/acidanthera/Lilu) - Needed for kexts such as AirportBrcmFixup and WhateverGreen to function.  
* [AirportBrcmFixup.kext](https://github.com/acidanthera/AirportBrcmFixup) - Allows 94352Z to work even though it's not native to MacOS.  
* [WhateverGreen](https://github.com/acidanthera/WhateverGreen) - Applies fixes for GPU
* [VirtualSMC](https://github.com/acidanthera/VirtualSMC) - Emulates the SMC and provides sensor information for Battery and CPU.
  * Includes SMCBatteryManager, SMCProcessor, and SMCSuperIO as well, which I use as well  
* [USBInjectAll](https://github.com/RehabMan/OS-X-USB-Inject-All) - Injects all the USB ports in SSDT-UIAC as macOS does not pick up all the USB Ports by itself.
* [VoodooPS2Controler]() - Fixes PS2 Trackpad and Keyboard (Not linked yet, I compiled my own based off of Dr. Hurtz and Rehabman's repo)
* [BrcmFirmwareRepo/BrcmPatchRAM2](https://github.com/RehabMan/OS-X-BrcmPatchRAM) - Allows the Bluetooth part of the 94352Z to work with macOS.
* [IntelMausiEthernet](https://github.com/Mieze/IntelMausiEthernet) - Fixes ethernet port.
* [AppleALC]() - Fixes speakers and 3.5mm ports

## Config.plist

### Patches

* _OSI to XOSI - Fakes Windows 8 by redirecting OSI calls to the one in the SSDT_XOSI file. This also includes versions before Windows 8 as the DSDT relies on behavior described [here](https://docs.microsoft.com/en-us/windows-hardware/drivers/acpi/winacpi-osi).  
* EHC1 to EH01/EHC2 to EH02 - Expected DSDT name for USBInjectAll
* ECDV to EC - Generated by [this script](https://github.com/corpnewt/USBMap)
* HPET _CRS to XCRS/IRQ 8 /IRQ 0 Patch - Generated by [this script](https://github.com/corpnewt/FixHPET), used with SSDT-HPET

### Droped Tables

* MCFG - Causes crashes with SSDT-CPUPM

### Boot

* radpg=15 - WhateverGreen power gating tweaks for Radeon GPUs
* shikigva=40/shiki-id - Fixes image previews
* Timeout = 0 - Does not show Clover GUI unless you hold down a button during boot

### Devices
* Pci(0x1,0x0) - Set dual link for 1080p Display, set 10bit panel, set connectors to connect to internal/external displays. [More Details](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.Radeon.en.md)  
The connectors is actually the Buri connectors from within the AMD 7000 Series kext, with the first connector modified to work with the internal LVDS display.  
* Pci(0x1b,0x0) - Set audio layout for AppleALC.kext to fix interal speakers and ports
* Pci(0x1c,0x7) - Fixes SD Card slot by allowing it to use the apple SD card kext within MacOS.

### Graphics
DO NOT INJECT - WhateverGreen handles all GPU stuff and injecting makes it so WhateverGreen can't put in the framebuffer and other fixes.

## SSDTs

* SSDT-CPUPM - Generated by [this script] (https://github.com/Piker-Alpha/ssdtPRGen.sh). Allows the CPU to turbo and lower it's clock speed.
* SSDT-HPET - Generated by [this script](https://github.com/corpnewt/FixHPET), fixes duplicate IRQs and fixes RTC
* SSDT-PNLF - From WhateverGreen, fixes backlight control
* SSDT-SDMMC - Adds the missing SD card device in the DSDT
* SSDT-UIAC - Used in conjunction with USBInjectAll, specifies which ports to be injected
* SSDT-XOSI - Fakes Windows 8

## Credits
* Thanks to everyone in r/Hackintosh's Discord Server. They helped out a ton.
* Authors of the Kexts, drivers, and scripts used.
