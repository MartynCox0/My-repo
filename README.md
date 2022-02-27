# My-repo

# This repo is intended to be a list of commands to input sequentially to install a base Arch Linux installation.

Link to video commands are taken : https://www.youtube.com/watch?v=4wbMcL1Optc

##Key maps
localectl list-keymaps | grep GB
load keys  [ name of key map you need ]

## Internet Connection
ip a
wifi-menu

### mirror list 
pacman -S reflector 
reflector -c UK -a 6 --sort rate --save /etc/pacman.d/mirrorlist
pacman -Syyy

## Disk Partitioning
lsblk
fsdisk /dev/sda
### Swap
n
p
1
2048
+16G
t
L
82

### root
n
p
2
default
default (take remainder of disk)
w

## Formatting Partitions 
mkswap /dev/sda1
swapon /dev/sda1
mkfs.ext4 /dev/sda2 
lsblk

## Mount Partition
mount /dev/sda2 /mnt

## Pacstrap
pacstrap /mnt base base-devel linux linux-firmware vim intel-ucode 

## UUID
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab

## Chroot into environment
arch-chroot /mnt 

## Timezone
timedatectl list-timezones | grep London
ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime
hwclock --systohc
vim /etc/locale.gen ------->  uncomment #en_GB
:wq
locale-gen
vim /etc/locale.conf  -----------> LANG=en_GB.UTF-8
:wq
vim /etc/vconsole.conf -----------> KEYMAP= [name of keymap from the start]
:wq

## Hostname
vim /etc/hostname -----------> [name of what you want to call your machine]
vim /etc/hosts ----append-------> 127.0.0.1   localhost 
               ----append-------> ::1   localhost 
               ----append-------> 127.0.1.1   [name of machine].localdomain   [name of machine]
               :wq
 
 # Password
 passwd 
 
 ## Grub install etc
 pacman -S grub networkmanager network-manager-applet wireless_tools wpa_supplicant dialog os-prober mtools dosfstools linux-headers bluez bluez-utils cups xdg-utils xdg-user-dirs 
 
 grub-install --target=i386-pc /dev/sda
 grub-mkconfig -o /boot/grub/grub.cfg
 
 ## Enabling Bluetooth
 systemctl enable bluetooth
 systemctl enable org.cups.cupsd
 systemctl enable NetworkManager
 
 ## add User
 useradd -mG wheel m
 passwd m
 EDITOR=vim visudo -----------> uncomment %wheel ALL=(ALL) ALL
 :wq
 
 ## SSH
 pacman -S openssh
 systemctl sshd 
 
 ## exit installer
 exit
 umount -a
 reboot
 
 ## graphics
 sudo pacman -S xf86-video-intel
 
