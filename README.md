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
```
```
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
```
Localization
```
vim /etc/locale.gen
en_GB.UTF-8 UTF-8
echo "LANG=en_GB.UTF-8" >> /etc/locale.conf
echo "KEYMAP=uk" >> /etc/vconsole.conf
```
Hostname
```
echo $hostname >> >> /etc/hostname
```
Hosts
```
vim /etc/hosts
127.0.0.1   localhost
::1         localhost
127.0.1.1   $hostname.localdomain   $hostname
```
Root password
```
passwd
```
Boot loader
```
pacman -S refind efibootmgr networkmanager network-manager-applet wireless_tools wpa_supplicant dialog mtools dosfstools linux-headers openssh
```
Install refind to boot partition
```
refind-install --usedefault /dev/sda1 --alldrivers
```
make refind config
```
mkrlconf
```
```
cd /boot/
```
ls should show
```
EFI  initramfs-linux-fallback.img  initramfs-linux.img  refind_linux.conf  vmlinuz-linux
```
```
vim refind_linux.conf
```
delete first two lines
ls
```
cd EFI
```
ls should show
```
BOOT
```
```
cd BOOT
```
ls should show
```
BOOT.CSV  bootx64.efi  drivers_x64  icons  keys  refind.conf
```
```
vim refind.conf
```
find arch linux section, edit options
```
options  "root=/dev/sda1  rw  add_efi_memmap"
```
Enable NM
```
systemctl enable NetworkManager
cd
```
Create user
```
useradd -mG wheel $username
passwd $username
```
```
EDITOR=vim visudo
uncomment to allow members of groupwheel ...
```
```
exit
reboot
```
