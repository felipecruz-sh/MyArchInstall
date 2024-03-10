# MyArchInstall

* Download Arch Linux: [Download](https://www.archlinux.org/download/)

* Create bootable USB drive
    * [Rufus](https://rufus.ie) 
    * [balenaEtcher](https://etcher.balena.io/#download-etcher) 
    * [Ventoy](https://www.ventoy.net/en/download.html)

### Set keyboard layout
```
# loadkeys br-abnt2
```

### Verify the boot mode
```
# cat /sys/firmware/efi/fw_platform_size
```
If the command returns 64, then system is booted in UEFI mode and has a 64-bit x64 UEFI. If the file does not exist, the system may be booted in BIOS or CSM mode