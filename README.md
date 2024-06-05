# Salon dotfiles


## [Install ArchLinux](https://wiki.archlinux.org/title/installation_guide)


[Connect to internet]()
```shell
iwctl
> station wlan0 scan
> station wlan0 connect "Canal de Nydus"
Ctrl+d

ping archlinux.org
```

Create partitions
```shell
fdisk -l
fdisk /dev/nvmen1
# Create GPT partition table
> g
# Create Boot partition (1G)
> n
>> enter
>> enter
>> +1G
>t 1
>> EFI System

# Create swap partition
> n
>> enter
>> enter
>> +nG
>t 2
>> Linux swap

# Create root partition
> n
>> enter
>> enter
>> +100G

# Create the home partition
> n
>> enter
>> enter
>> enter

# Write to disk. This is destructive operation
``̀

Write Filesystem on partitions
```shell
mkfs.fat -F 32 /dev/nvme0n1p1
mkswap /dev/nvme0n1p2
mkfs.btfs /dev/nvme0n1p3
mkfs.btfs /dev/nvme0n1p4
```

Mount partitions
```shell
mount --mkdir /dev/nvme0n1p3 /mnt
mount --mkdir /dev/nvme0n1p1 /mnt/boot
mount --mkdir /dev/nvme0n1p4 /mnt/home
```

Init swap partition if exists
```shell
swapon /dev/nvme0n1p2
```

Install packages
```shell
pacstrap -K /mnt base linux linux-firmware sof-firmware vim intel-ucode networkmanager gnome git base-devel sudo bluez bluez-utils seahorse
```

Create FS stab
```shell
genfstab -U /mnt /mnt/etc/fstab
```

Chroot
```shell
arch-chroot /mnt
```

Set timezone
```shell
ln -sf /usr/share/zoneinfo/Canada/Eastern /etc/localtime
```

Set locale
```shell
vim /etc/locale.gen # uncomment ca_FR
locale-gen
```

Set hostname
```shell
vim /etc/hostname
```

Enable services
```shell
systemctl enable NetworkManager # Enable internet
systemctl enable gdm # Enable Gnome
systemctl enable bluetooth # Enable Bluetooth
```

Create users

```shell
useradd -m salon
passwd salon
visudo 

```

Quit chroot
```shell
exit
```

Set Root password
```shell
passwd
```

Create Boot Loader
```shell
cd
curl -o aufii https://raw.githubusercontent.com/de-arl/auto-UEFI-entry/master/aufii
chmod +x aufii
./aufii
>> /dev/nvme0n1p1
>> i
>> l
>> ce
```

Reboot
```shell
umount -R /mnt
reboot
```

Connect to internet : https://wiki.archlinux.org/title/NetworkManager
```shell
nmcli device wifi connect <wifi> password <password>
```

Install YaY
```shell
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```

Install Brave
```shell
sudo flatpak install flathub com.brave.Browser
# or use the software
```
Configure Brave
- Remove the hardward acceleration in Settings/System

Install nordvpn
```
yay -S nordvpn-bin
```

Configure nordvpn
```
sudo usermod -aG nordvpn $USER
systemctl enable nordvpnd
sudo reboot
nordvpn login
nordvpn set killswitch on
#nordvpn whitelist add subnet 192.168.0.1/24
nordvpn set lan-discovery on
nordvpn set autoconnect on
```

Install WebTorrent
```shell
yay -Sy webtorrent-desktop
```

Enable RDP
```shell
grdctl enable rdp
grdctl set-credentials salon <password>
grdctl enable vnc
grdctl set-password <password>
```

# Configure Ubuntu
## What did I do ?

### Install Brave
```
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list
sudo apt update
sudo apt install brave-browser
```

### Configure Brave

 Set brave to not use torrents


### Install nordvpn
```
sh <(curl -sSf https://downloads.nordcdn.com/apps/linux/install.sh)
```

### Configure nordvpn
```
nordvpn login
sudo usermod -aG nordvpn $USER
sudo reboot
nordvpn set killswitch on
nordvpn whitelist add subnet 192.168.0.1/24
nordvpn set autoconnect on
```

### Download WebTorrent
```
(
    cd ~/Downloads
    wget https://github.com/webtorrent/webtorrent-desktop/releases/download/v0.24.0/webtorrent-desktop_0.24.0_amd64.deb
    sudo dpkg -i webtorrent-desktop_0.24.0_amd64.deb
)
```

### Install vlc
```
sudo snap install vlc
```

### Install openssh-server
```
sudo apt update
sudo apt install -y openssh-server
```

### 
