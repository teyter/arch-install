Load UK keyboard
```
loadkeys uk
```
Verify boot mode
```
ls /sys/firmware/efi/efivars
```
Internet ?
```
ip link / wifi-menu
```
Update system clock
```
timedatectl set-ntp true
```
partition disk
```
fdisk -l
fdisk /dev/sda
n
+500M
t
1 EFI boot partition
n
+(RAM*1.5)G
t
19 linux swap partition
n
enter enter enter
w
```
Make filesystem
```
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda4
mkswap /dev/sda2
swapon /dev/sda2

```
Mount filesystem
```
mount /dev/sda3 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```
lsblk should display
```
sda
-sda1   500M    part    /mnt/boot
-sda2   12G     swap    swap
-sda3   200G    part    /mnt
```
letsgo pacstrap
```
pacstrap /mnt base base-devel linux linux-firmware vim
```
Fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```
Chroot
```
arch-chroot /mnt
```
Time zone
```
ln -sf /usr/share/zoneinfo/Iceland /etc/localtime
hwclock --systohc
