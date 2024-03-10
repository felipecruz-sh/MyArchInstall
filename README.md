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

### Check internet connection
```
# ping -c 4 archlinux.org
```

### Disk partitioning
```
# cfdisk /dev/sdd
```
| Device    | Mount         | Size  | Type              |
| :-------: | :-----------: | :---: | :---------------: |
| /dev/sdd1 | `/boot/efi`   | 1G    | EFI System        |
| /dev/sdd2 | `/`           | 50G   | Linux root (ext4) |
| /dev/sdd3 | `/home`       | 187.5 | Linux home (ext4) |

### Format partitions
```
# mkfs.fat -F32 /dev/sdd1

# mkfs.ext4 /dev/sdd2

# mkfs.ext4 /dev/sdd3
```

### Mount the file systems
```
mount /dev/sdd2 /mnt

mount --mkdir /dev/sdd1 /mnt/boot/efi

mount --mkdir /dev/sdd3 /mnt/home
```

### Install essential packages
```
# pacstrap -K /mnt base linux linux-firmware nano networkmanager grub grub-efi-x86_64 efibootmgr
```

### Fstab
```
# genfstab -U /mnt >> /mnt/etc/fstab
```

### Chroot
```
# arch-chroot /mnt
```

### Time
```
# ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

# hwclock --systohc
```

