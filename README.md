# Salon dotfiles

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
