---
title: ArchLinux Installation and Configuration
date: 2022-09-05 19:36:14
tags:
- Linux
- DIY
categories:
- Linux
---
# ArchLinux

## Installation

1. Download the [ArchLinux ISO file](https://archlinux.org/download/) and use [Rufus](https://rufus.ie/) to write it to a USB drive.

2. Go to BIOS setting and change the boot sequence.

3. Boot the live environment.

4. Connect to the internet using iwctl.
   ```bash
   $ iwctl
   ```

   ```iwd
   [iwd]# device list
   [iwd]# station device_name scan
   [iwd]# station device_name get-networks
   [iwd]# station device_name connect Network_Name
   ```

5. Update system time
   ```bash
   $ timedatectl set-ntp true
   ```

6. Partition the disk using `cfdisk`

   | Partition |      Space      |
      | :-------: | :-------------: |
   |   boot    |       1G        |
   |   swap    |       1G        |
   |   root    | All rest spaces |

7. Format partitions
   ```bash
   $ mkfs.ext4 /dev/root_partition
   $ mount /dev/root_partition /mnt
   $ mkswap /dev/swap_partition
   $ swapon /dev/swap_partition
   $ mkfs.fat -F 32 /dev/efi_system_partition
   $ mount --mkdir /dev/efi_system_partition /mnt/boot
   ```

8. Start installation
   ```bash
   $ pacstrap /mnt base linux linux-firmware neovim fish networkmanager man-db man-pages
   $ genfstab -U /mnt >> /mnt/etc/fstab
   ```

9. Chroot into the system and configure it
   ```bash
   $ arch-chroot /mnt
   $ ln -sf /use/share/zoneinfo/region /etc/localtime
   $ hwclock --systohc
   $ vim /etc/locale.gen
   ```

   ```vim
   en_US.UTF-8 UTF-8
   zh_CN.UTF-8 UTF-8
   ```

   ```bash
   $ locale-gen
   $ vim /etc/locale.conf
   ```

   ```vim
   LANG=zh_CN.UTF-8
   ```

   ```bash
   $ vim /etc/hostname
   ```

   ```vim
   brandenxia
   ```

   ```bash
   $ mkinitcpio -P
   $ passwd
   $ useradd -m -G wheel -s /usr/bin/fish brandenxia
   $ passwd brandenxia
   $ visudo
   $ systemctl enable NetworkManager
   $ exit
   $ umount /mnt/boot
   $ umount /mnt
   $ shutdown -h now
   ```
## Configuration

1. Install the desktop
   ```bash
   user login: brandenxia
   password:
   $ sudo pacman -S xorg
   $ sudo pacman -S i3-gaps
   ```

2. Config the desktop

    ```bash
    # copy config from github
    $ git clone https://github.com/BrandenXia/configs-dotfile ~/.config
    # install necessary software
    $ sudo pacman -S rofi ranger neofetch i3status alacritty fcitx5 flameshot pasystray nitrogen parcellite firefox
    # install font
    $ sudo pacman -S ttf-roboto ttf-roboto-mono
    # install aur manager
    $ sudo pacman -S paru
    # install i3lock-fancy from aur
    $ paru -S i3lock-fancy
    ```

3. Enter desktop environment
    ```bash
    # start desktop
    $ startx "/usr/bin/i3"
    ```

4. Finished
