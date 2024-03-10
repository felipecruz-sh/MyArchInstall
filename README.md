# MyArchInstall

* Download Arch Linux: [Download](https://www.archlinux.org/download/)

* Create bootable USB drive
    * [Rufus](https://rufus.ie) 
    * [balenaEtcher](https://etcher.balena.io/#download-etcher) 
    * [Ventoy](https://www.ventoy.net/en/download.html)

### Set keyboard layout
```sh
# loadkeys br-abnt2
```

### Verify the boot mode
```sh
# cat /sys/firmware/efi/fw_platform_size
```
If the command returns 64, then system is booted in UEFI mode and has a 64-bit x64 UEFI. If the file does not exist, the system may be booted in BIOS or CSM mode

### Check internet connection
```sh
# ping -c 4 archlinux.org
```

### Disk partitioning
```sh
# cfdisk /dev/sdd
```
| Device    | Mount         | Size  | Type              |
| :-------: | :-----------: | :---: | :---------------: |
| /dev/sdd1 | `/boot/efi`   | 1G    | EFI System        |
| /dev/sdd2 | `/`           | 50G   | Linux root (ext4) |
| /dev/sdd3 | `/home`       | 187.5 | Linux home (ext4) |

### Format partitions
```sh
# mkfs.fat -F32 /dev/sdd1

# mkfs.ext4 /dev/sdd2

# mkfs.ext4 /dev/sdd3
```

### Mount the file systems
```sh
mount /dev/sdd2 /mnt

mount --mkdir /dev/sdd1 /mnt/boot/efi

mount --mkdir /dev/sdd3 /mnt/home
```

### Install essential packages
```sh
# pacstrap -K /mnt base linux linux-firmware nano networkmanager grub grub-efi-x86_64 efibootmgr
```

### Generate Fstab
```sh
# genfstab -U /mnt >> /mnt/etc/fstab
```

### Chroot
```sh
# arch-chroot /mnt
```

### Time
```sh
# ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

# hwclock --systohc
```

### Localization
```sh
# sed -i '/en_US/,+1 s/^#//' /etc/locale.gen

# locale-gen

# echo LANG=en_US.UTF-8 > /etc/locale.conf

# export LANG=en_US.UTF-8

# echo KEYMAP=br-abnt2 > /etc/vconsole.conf
```

### Network
```sh
echo hostname > /etc/hostname

cat > /etc/hosts << EOF
127.0.0.1   localhost.localdomain   localhost
::1         localhost.localdomain   localhost
127.0.0.1   hostname.localdomain    hostname
EOF
```

### Root password
```sh
# passwd
```

### Admin User
```sh
# useradd -mG wheel username

# passwd
```

### Boot loader
```sh
# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch --recheck

# grub-mkconfig -o /boot/grub/grub.cfg
```

### Services
```sh
# systemctl enable NetworkManager.service
```

### Reboot
Exit chroot environment by pressing `Ctrl + D` or typing `exit`

Unmount system mount points:
```sh
# umount -R /mnt
```

Reboot system:
```sh
# reboot
```