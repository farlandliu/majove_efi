# Majove installation for Lenovo ZHAOYANG (联想昭阳) E42-80  I7-7500U @ 2.7GHZ HD620
This repo is  Majove installation config files for Lenovo ZHAOYANG E42-80 I7-7500U 2.7MHZ

## Bios Settings
- SATA MODE: AHCI
- Graphic Device: UMA only
- Intel Virtual Technology: Disabled
- Boot Mode: Legacy Support
- Boor Priority: UEFI first

## Prepare Clover booting USB

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

  - The Clover installer options is as the follow:
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
    - copy HFSPlus.efi to drivers/UEFI, otherwise can't find the majove installer partition in the USB
       https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi

- Prepare Kexts in the `other` folder :
  - FakeSMC.kext
  - USBInjectAll.kext
  - GenericUSBXHCI.kext
  - Lilu.kext
  - WhateverGreen.kext
  - SATA-unsupported.kext
  - **VoodooTSCSync.kext**
    > This the most import kext for lenovo E42-80, if no this kext, the laptop can't boot into the macOS installer, will froze at the loading bar comes out(boot with args -v). credit @hswwm, refer this [post](http://bbs.pcbeta.com/viewthread-1796587-1-1.html) at the chinese language forum.

    > **Issue description**: 
    when trying to install majove on E42-80, mostly the system can't boot into the installing phase. That was when you finished the installation pahse 1,  system will frozen when the loading bar comes out after rebooting. Even reboot, it will still froze at the loading screen. 
    I've tried several ways to fix the problem,  including injecting the fake graphic id, changing the boot args with -f(hopes clear the cache). Sometimes, when I changed the config file, maybe can finished the installing phase 1, but frozed again after reboot. 
    The issue finally fixed by add this kext: VoodooTSCSync.kext.


- Choosing a config.plist:
  select "config_UHD630.plist" from https://github.com/RehabMan/OS-X-Clover-Laptop-Config/raw/master/config_UHD630.plist

  values edited from the raw plist:
  > ref: Problems with Kaby Lake Graphics
https://www.tonymacx86.com/threads/readme-common-problems-and-workarounds-on-10-14-mojave.255823/
    - boot args: dart=0  -cdfon lilucpu=8  debug=0x100 -v
    - themes: Clovy
    - boot - Legacy: PBR
    - Graphics - Inject - ATI= NO, Nvidia=No, Intel=No
    - AAPL,ig-platform-id= 00001B59 , device-id=16590000 




