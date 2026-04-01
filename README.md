# macOS on a Dell Optiplex 3080 Micro
This is an macOS EFI for a Dell Optiplex 3080 Micro.

System specs
-
- i7 10700T/UHD 630
- 16GB DDR4
- 512GB PM991a NVMe
- BCM94360CS2
- RTL8111HSD-CG Ethernet
- 65W adapter
- ALC256 codec
- 2.34.0 BIOS



**Tested on macOS Tahoe 26.4**

What's supported
-
- Sleep/Shutdown/Restart
- AirDrop
- Continuity Camera
- Universal Clipboard
- Universal Control
- Watch Unlock/Authentication
- Wired Sidecar
- Handoff
- AirPlay
- Audio (internal, jacks, HDMI/DP)
- USB ports
- Find My iMac


Known caveats/issues
-
- Wireless Sidecar render issues
- Slow Watch Unlock on wake (caused by AppleALC loading delay on wake)
- powernap causes sleep failure

Kexts
-
- [Lilu.kext](https://github.com/acidanthera/lilu/releases)
- [VirtualSMC.kext](https://github.com/acidanthera/virtualsmc/releases)
- [NVMeFix.kext](https://github.com/acidanthera/NVMeFix/releases)
- [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)
- [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases/)
- [RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
- [SMCProcessor.kext](https://github.com/acidanthera/virtualsmc/releases)
- [SMCSuperIO.kext](https://github.com/acidanthera/virtualsmc/releases)
- [USBToolBox.kext](https://github.com/USBToolBox/kext/releases)
- [UTBMap.kext (generate in Windows through USBToolBox tool script)](https://github.com/USBToolBox/tool/releases)
- [CPUFriend.kext](https://github.com/acidanthera/cpufriend)
- [CPUFriendDataProvider.kext](https://github.com/acidanthera/cpufriend)
- [AMFIPass.kext](https://github.com/bluppus20/AMFIPass/releases)
- [BlueToolFixup.kext](https://github.com/acidanthera/BrcmPatchRAM/releases/)
- [RestrictEvents.kext](https://github.com/acidanthera/RestrictEvents/releases)


AX200-specific (stock wireless card kexts):
- [itlwm.kext](https://github.com/openintelwireless/itlwm/releases)
- [Airportitlwm.kext](https://github.com/openintelwireless/itlwm/releases) (itlwm substitute for partial Continuity features and native Wi-Fi interface, requires patching)
- [IntelBluetoothFirmware.kext](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)
- [IntelBTPatcher.kext](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)


BCM94360CS2-specific:
- [IOSkywalkFamily.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/tree/main/payloads/Kexts/Wifi)
- [IO80211FamilyLegacy.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/tree/main/payloads/Kexts/Wifi)
- [AirPortBrcmNIC.kext (bundled inside IO80211FamilyLegacy.kext)](https://github.com/dortania/OpenCore-Legacy-Patcher/tree/main/payloads/Kexts/Wifi)



Tools
-
- [Rufus](https://rufus.ie/en/)
- [ru.efi](https://ruexe.blogspot.com/)
- [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools/releases)
- [Hackintool](https://github.com/benbaker76/Hackintool/releases)
- [SSDTTime](https://github.com/corpnewt/ssdttime)
- [MaciASL](https://github.com/acidanthera/MaciASL/releases)
- [OCLP](https://github.com/dortania/Opencore-Legacy-Patcher/releases) / [OCLP-Mod](https://github.com/laobamac/OCLP-Mod/releases)
- [USBToolBox](https://github.com/USBToolBox/tool/releases)
- [OpCoreSimplify](https://github.com/lzhoang2801/OpCore-Simplify)


Software-sided changes
-
- OCLP (or OCLP-Mod for Tahoe)
- Disabling FileVault (for a native lock screen)
- "_sudo pmset -a standby 0 womp 1 proximitywake 0 powernap 0 networkoversleep 1 disksleep 0 hibernatemode 0 tcpkeepalive 1_" on Terminal


Recommended BIOS Modifications
-

GUI settings
- TPM -> Disabled
- Secure Boot -> Disabled
- SMM Security Mitigation -> Disabled
- SATA Operation -> AHCI
- Intel SGX -> Disabled
- ASPM -> Off


Hidden settings (through ru.efi or similar)
- CFG Lock -> Disabled
- DVMT Pre-Allocated -> 64MB
- PCI Express Clock Gating -> Disabled (recommended on BCM94360CS2 to prevent network performance degradation on S3->S0)

Note: If you are on a different BIOS version, I recommend dumping/finding your exact BIOS and extracting a readable text file from it.

Exact BIOS Values (2.34.0)
-
| Setting | UEFI Variable | Address | Default | Replacement Value |
| ------- | ------------- | ------- | ------- | ------------------|
| CFG Lock | CpuSetup | 0x3E | 1 | 0 |
| DVMT Pre-Allocated | SaSetup | 0xF5 | 1 | 2 |
| PCI Express Clock Gating | PchSetup | 0xB2 | 0 | 1 |

This EFI has been made for educational purposes.
**You must generate your own serial numbers if you decide to clone this EFI**
