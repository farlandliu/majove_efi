# Majove installation for Lenovo ZHAOYANG (联想昭阳) E42-80  I7-7500U @ 2.7GHZ
This repo is  Majove installation config files for Lenovo ZHAOYANG E42-80 I7-7500U 2.7MHZ

## Bios Settings
- SATA MODE: AHCI
- Graphic Device: UMA only
- Intel Virtual Technology: Disabled
- Boot Mode: Legacy Support
- Boor Priority: UEFI first

## Prepare Clover boot USB

- Clover EFI v2.4k_r4988
  https://sourceforge.net/projects/cloverefiboot/files/Installer/Clover_v2.4k_r4988.zip/download
- installing clover to usb
  As Rehabman's recommendation in the laptop [installation guide](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/), I used the option 1: FAT32 + HFS+J partitions, convenient for mounting the EFI folder.

  The completed Majove booting USB is:

  ```
  > diskutil list
  ...
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.5 GB    disk2
   1:                 DOS_FAT_32 CLOVER EFI              209.7 MB   disk2s1
   2:                  Apple_HFS Install macOS Mojave    15.3 GB    disk2s2
  ```

  The clover installer options is as the follow:

  > For Clover legacy, run the Clover Installer package:
  - select the target of the install to "CLOVER EFI" using "Change Install Location"
  - select "Customize" (we need to change some of the default options)
  - "Install for UEFI booting only": unchecked
  - "Install Clover in the ESP": unchecked
  - Dirvers off: checked
  - Boot Sectors: 
      - install boot0af in MBR: checked
  - Clover for BIOS legacy booting
      - Clover EFI 64-bits SATA
  - UEFI drivers
      - File system drivers: checked all
  - Themes: Clovy

- Prepare the other Kexts:
  - FakeSMC.kext
  - USBInjectAll.kext
  - GenericUSBXHCI.kext
  - Lilu.kext
  - WhateverGreen.kext
  - SATA-unsupported.kext
  - **VoodooTSCSync.kext**
    This the most import kext for lenovo E42-80, if no this kext, the laptop can't boot into the macOS installer, will be stuck at the logo screen or phase 2 installing screen.

- Choosing a config.plist:
  select "config_HD615_620_630_640_650.plist" from https://github.com/RehabMan/OS-X-Clover-Laptop-Config
  
