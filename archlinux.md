- start archlinux bootable media on virtualbox
- Add usb device
- Loadkeys fr-latin1
- timedatectl set-ntp true
- fdisk -l  # check device entry point /dev/sdX
- cfdisk /dev/sdX to partition
    - 1st partition : /boot
    - 2nd partition : 15G /
    - 3rd partition : 10G /shared
    - 4th partition : remaining /home
- mkfs.ntfs /dev/sdX3
- mkfs.fat -F32 /dev/sdx1
https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_a_non-root_file_system#Partition
- cryptsetup -v --key-size 512 --hash sha512 --use-random luksFormat /dev/sdX2
XXXXXXXXXXXXX
- cryptsetup -v --key-size 512 --hash sha512 --use-random luksFormat /dev/sdX4
Verify with cryptsetup luksDump device
Open device with cryptsetup open --type luks device dm_name
Parition is then available in /dev/mapper/dm_name
Close device with cryptsetup close dm_name
- mkfs.ext4 -O "^has_journal" /dev/
- mkfs.ext4 -O "^has_journal" /dev/sdX4
- mount /dev/mapper/dm_name1 /mnt
- mkdir /mnt/home
- mount /dev/mapper/dm_name2 /mnt/home
- mkdir /mnt/shared
- mount /dev/sdX3 /mnt/shared
- mkdir /mnt/boot
- mount /dev/sdX1 /mnt/boot
- pacstrap /mnt base base-devel
- genfstab -U /mnt >> /mnt/etc/fstab
- Arch-chroot /mnt
- edit file /etc/crypttab
add entry for home partition :
crypthome   UUID=<lsblk -f to get UUID of crypted device>   none    timeout=45
check that /etc/fstab uses the correct home partition (/dev/mapper/crypthome)
- ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
- hwclock --systohc
- nano /etc/locale.gen
locale-gen
- nano /etc/locale.conf
LANG=en_US.UTF-8
- nano /etc/vconsole.conf
KEYMAP=fr-latin1
- (localectl set-keymap --no-convert fr-latin1)
- nano /etc/mkinitcpio.conf
add "block" right after "udev" hook
add "encrypt" before "filststems"
add "keymap keyboard" before "encrypt"
pacman -S intel-ucode

- mkinitcpio -p linux
- passwd
- systemctl enable dhcpcd
- pacman –S ntfs-3g
- pacman –S iw wpa_supplicant
- pacman –S grub
- pacman –S efibootmgr
- grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub 
- grub tuning
nano /etc/default/grub
GRUB_CMD_LINE_LINUX_DEFAULT = "quiet nouveau.modeset=0 acpi_osi=Linux acpi_osi='!Windows 2015'"
GRUB_CMD_LINE_LINUX = "cryptdevice=/dev/sdX2:cryptroot"
- grub config windows
grub-probe --target=hints_string /boot/EFI/Microsoft/Boot/bootmgfw.efi 
grub-probe --target=fs_uuid /boot/EFI/Microsoft/Boot/bootmgfw.efi 
nano /boot/grub/custom.cfg
if [ "${grub_platform}" == "efi" ]; then
        menuentry "Microsoft Windows 10 UEFI-GPT" {
                insmod part_gpt
                insmod fat
                insmod search_fs_uuid
                insmod chain
                search --fs-uuid --set=root --hint-bios=hd0,gpt2 --hint-efi=hd0,gpt2 --hint-baremetal=ahci0,gpt2 $fs 14BA-D888
                chainloader /EFI/Microsoft/Boot/bootmgfw.efi
        }
fi

grub-mkconfig -o /boot/grub/grub.cfg



## WIFI
- https://wiki.archlinux.org/index.php/Wireless_network_configuration
- check if device in the list and if a driver is usedlspci
lspci -k
- ip link to check if a wireless interface was created (wlp3s0)
- ip link set wlp3s0 up
### setup
- get interface name
iw dev
=> wlp3s0
- ip link set wlp3s0 up

- scan networks :
iw dev wlp3s0 scan | less

- create /etc/wpa_supplicant/wpa_supplicant-wlp3s0.conf
ctrl_interface=/var/run/wpa_supplicant
network={
    ssid="Livebox-A59A"
    psk="tbd"
    key_mgmt=WPA-PSK
    priority=1
}

- test with wpa_supplicant -i wlp3s0 -c /etc/wpa_supplicant/wpa_supplicant-wlp3s0.conf
- systemctl enable wpa_supplicant@wlp3s0

- https://wiki.archlinux.org/index.php/Dhcpcd
- ln -s /usr/share/dhcpcd/hooks/10-wpa_supplicant /usr/lib/dhcpcd/dhcpcd-hooks/
systemctl enable dhcpcd.service

## CARTE GRAPHIQUE
- list hardware
lspci -k | grep -A 2 -E "(VGA|3D)"
> GM204M (geforce gtx 970M rev a1) subsystem CLEVO/KAPOK Computer device 6540
- Get GPU code name  :
https://nouveau.freedesktop.org/wiki/CodeNames/#NV110
> NV124
- pacman -S nvidia
libglvnd

- nvidia-xconfig
Edit the generated file & check
add BoardName "GeForce GTX 970M (rev a1)"

- https://wiki.archlinux.org/index.php/Clevo_P650RS
> BIOS Option

## TODO : GNOME
pacman -S gnome

systemctl enable gdm.service

pacman -S sudo

visudo

useradd -m jolo
groupadd sudo
gpasswd -a jolo sudo


## Wallpaper
install fcrontab
fcrontab -e
SHELL=/bin/bash

# Change wallpaper every five minutes
@ 5 /home/jolo/.config/change-background.sh ; /bin/true

change-background.sh:
#!/bin/bash

PID=$(pgrep -u `whoami` ^gnome-shell$)
export DBUS_SESSION_BUS_ADDRESS=$(grep -z DBUS_SESSION_BUS_ADDRESS /proc/$PID/environ|cut -d= -f2-)

DIR="$HOME/Pictures/wallpapers"
#GCONF="gsettings set org.gnome.desktop.background picture-uri"
PIC=$(ls $DIR/* | shuf -n1)
gsettings set org.gnome.desktop.background picture-uri \'file://${PIC}\'


# System maintenance
# https://wiki.archlinux.org/index.php/System_maintenance

Check errors
systemctl --failed
journalctl -p 3 -xb

mkdir /opt/backup
mkdir /opt/backup/$(date '+%Y-%m-%d')

- List of installed packaged
cd /opt/backup/$(date '+%Y-%m-%d')
pacman -Qqe > ./pkglist.txt

- Backup of pacman db 
tar -cjf /opt/backup/$(date '+%Y-%m-%d')/pacman_database.tar.bz2 /var/lib/pacman/local

- Backup specific files
tar -czpvf /opt/backup/$(date '+%Y-%m-%d')/specific-files.tar.gz /home /etc /var

- restore pacamn db (in /)
tar -xjvf pacman_database.tar.bz2




#!/bin/bash

echo Beginning system update
mkdir /opt/backup -p
mkdir /opt/backup/$(date '+%Y-%m-%d')

# List of installed packaged
cd /opt/backup/$(date '+%Y-%m-%d')
pacman -Qqe > /opt/backup/$(date '+%Y-%m-%d')/pkglist.txt

# Backup of pacman db
tar -cjf /opt/backup/$(date '+%Y-%m-%d')/pacman_database.tar.bz2 /var/lib/pacman/local

# Backup specific files
tar -czpf /opt/backup/$(date '+%Y-%m-%d')/specific-files.tar.gz /home /etc /var

pacman -Syu

rm -rf /opt/build
mkdir /opt/build -p
cd /opt/build

git clone https://aur.archlinux.org/firefox-developer.git
cd firefox-developer
makepkg -sic

git clone https://aur.archlinux.org/gnome-terminal-transparency.git
cd gnome-terminal-transparency
makepkg -sic





















