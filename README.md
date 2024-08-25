# Arch Linux Installation (WIP)

https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup#arch

https://wiki.archlinux.org/title/REFInd#Using_shim

### Set the console keyboard layout

```
localectl list-keymaps
```
```
loadkeys uk
```

### Connecting to Network

`# iwctl`
<br>
`[iwd]# device list`
<br>
`[iwd]# station name connect SSID`
<br>
Here name is device name from the previous command

### Partition disks

`# cfdisk /dev/<diskname>`
<br>
`# mkfs.ext4 /dev/root_partition`
<br>
`# mkfs.fat -F 32 /dev/efi_system_partition`
<br>
`# mount /dev/root_partition /mnt`
<br>
`# mount --mkdir /dev/efi_system_partition /mnt/boot`

### Installing the base system

```
pacstrap -K /mnt linux base base-devel linux-firmware git refind os-prober efibootmgr linux-firmware-marvell networkmanager micro intel-ucode pipewire wireplumber xorg zsh zsh-autosuggestions zsh-completions zsh-syntax-highlighting fzf intel-media-driver libvdpau-va-gl libva-utils vdpauinfo intel-media-sdk thermald wezterm sddm plasma-meta
```

### Generating the filesystem table

```
genfstab -U /mnt >> /mnt/etc/fstab
```

### Changing root into the new system

```
arch-chroot /mnt
```

### Setting time

```
ln -sf /usr/share/zoneinfo/_Region_/_City_ /etc/localtime
```
```
hwclock --systohc
```

### Localization

```
echo "en_GB.UTF-8 UTF-8" > /etc/locale.gen
```
```
locale-gen
```
```
echo "LANG=en_GB.UTF-8" > /etc/locale.conf
```
```
echo "KEYMAP=uk" > /etc/vconsole.conf
```

### Creating hostname

```
hostnamectl set-hostname surface
```
```
micro /etc/hosts
```
then type in the following
```
127.0.0.1	localhost
::1		localhost
127.0.1.1	surface.localdomain	surface
```

### Creating User

```
passwd
```
```
useradd -m -G wheel -s /bin/zsh rafi
```
```
passwd rafi
```
```
EDITOR=micro visudo
```
then uncomment %wheel ALL=(ALL) ALL

### Enabling Network

```
systemctl enable NetworkManager
```

### Installing YAY

```
git clone https://aur.archlinux.org/yay-bin.git && cd yay-bin && makepkg -si
```

### Installing rEFInd Bootloader

```
yay -S ttf-ms-win11 ttf-freebanglafont libwacom-surface shim-signed
```
```
refind-install --alldrivers --shim /usr/share/shim-signed/shimx64.efi
```

### Enabling Display Manager

```
systemctl enable sddm.service
```