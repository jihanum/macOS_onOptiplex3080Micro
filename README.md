# macOS on a Dell Optiplex 3080 Micro
This is an macOS EFI specialized to work on a Dell Optiplex 3080 Micro. It contains the config.plist and the files associated with it.
- Note: This EFI has been made for educational purposes, and I have intended to create this EFI purely to learn about low-level hardware interactions with the OS.

System specs
-
- Intel i7 10700T
- UHD 630 Integrated Graphics
- 16GB 2933MHz DDR4
- 512GB PM991a NVMe
- Broadcom 94360CS2 Wi-Fi/BT Combo Network Card (For full functionality compared to the original Intel AX200NGW)
- RTL8111HSD-CG 1Gbe Ethernet
- 65W PSU
- ALC256 (ALC 3246) audio codec
- BIOS Version: 2.34.0



**Tested on macOS Tahoe 26.4**

What works
-
- Sleep/Shutdown/Restart
- AirDrop (two-way)
- Continuity Camera
- Universal Clipboard
- Universal Control
- Watch Unlock/Authentication
- Wired Sidecar
- Handoff (e.g. Safari Tabs, Discord)
- AirPlay Receiver
- Internal speakers, both audio jacks, HDMI/DP Audio
- All USB ports


Known caveats/issues
-
- Nonfunctioning hibernate
- powernap/tcpkeepalive/networkoversleep cause sleep instability on second sleep
- Wireless Sidecar doesn't render
- Slight delay on Watch Unlock upon wake due to AppleHDA audio loading
- No Phone Mirroring (lack of T2 chip)
- Find My on sleep (since tcpkeepalive is disabled)
- Trim is disabled in config.plist (due to slow boot times on the PM991a)


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


AX200-specific:
- [itlwm.kext](https://github.com/openintelwireless/itlwm/releases)
- [Airportitlwm.kext](https://github.com/openintelwireless/itlwm/releases) (itlwm substitute for partial Continuity features and native Wi-Fi interface, requires patching)
- [IntelBluetoothFirmware.kext](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)
- [IntelBTPatcher.kext](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)


Broadcom-specific:
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


Software-sided patches
-
- OCLP (or OCLP-Mod for Tahoe)
- Disabling FileVault (for a native lock screen)
- "_sudo pmset -a standby 0 womp 0 proximitywake 0 powernap 0 networkoversleep 0 disksleep 1 sleep 1 hibernatemode 0 tcpkeepalive 0 acwake 0_" on Terminal
- sleepwatcher (close and open apps that crash or slow down sleep, such as WhatsApp)


Recommended BIOS Modifications
-

Through the GUI
- TPM -> Disabled
- Secure Boot -> Disabled (unless keys are created for macos efis)
- SMM Security Mitigation -> Disabled
- SATA Operation -> AHCI
- PTT On -> Disabled
- Intel SGX -> Disabled
- ASPM -> Off


Through ru.efi
- CFG Lock -> Disable (or enable AppleXcpmCfgLock)
- DVMT Pre-Allocated -> 64MB (or use framebuffer_stolenmem flag on the GPU)
- Wake on WLAN and BT Enable -> 1 (especially useful if you want to wake using Bluetooth peripherals)
- PCI Express Clock Gating -> Disabled (recommended if you are using the BCM94360CS2, since S3 sleep causes network performance degradation on wake)
(I recommend dumping your BIOS or finding your exact version of your BIOS online and extracting it into text, to analyze and find the exact flag ids and offsets.)

Exact BIOS Values (2.34.0)
-
| Setting | UEFI Variable | Address | Default | Replacement Value |
| ------- | ------------- | ------- | ------- | ------------------|
| CFG Lock | CpuSetup | 0x3E | 1 | 0 |
| DVMT Pre-Allocated | SaSetup | 0xF5 | 1 | 2 |
| Wake on WLAN and BT Enable | PchSetup | 0x0D | 0 | 1 |
| PCI Express Clock Gating | PchSetup | 0xB2 | 0 | 1 |
