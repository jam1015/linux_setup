---
Title: Linux config
Author: Jordan Mandel
---

# Network Configuration

## on actual system

Add to `/etc/sysctl.d/40-ipv6.conf`

add network interfaes from `ip link show`

```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
net.ipv6.conf.enp0s25.disable_ipv6 = 1
net.ipv6.conf.wlp3s0.disable_ipv6 = 1
```


then apply with `sysctl -p /etc/sysctl.d/40-ipv6.conf`. I think this changes kernel parameters.

then in `/etc/ssh/sshd_config` add `AddressFamily inet` to specify ipv4 for ssh.

log onto network with `nmtui` or `iwctl`

having trouble disabling it permanently.

## on iso

Issue the command `sysctl -w net.ipv6.conf.all.disable_ipv6=1`
Issue the command `sysctl -w net.ipv6.conf.default.disable_ipv6=1`

or change `/etc/sysctl.conf`

```
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
net.ipv6.conf.lo.disable_ipv6=1
```
followed by `sudo sysctl -p`

```
[iwd]# device list
[iwd]# station device scan
[iwd]# station device get-networks
[iwd]# station device connect SSID
```

## Set hostnames
to `/etc/hosts`

add

```
127.0.0.1 localhost
127.0.1.1 myhostname
```

# Make sure proper firmware is installed
for example `sudo pacman -S linux-firmware`

# Graphics Drivers

`sudo pacman -S nvidia nvidia-utils`
`sudo pacman -S mesa`
`sudo pacman -S intel-ucode`

consider nvidia-390xx-utils vs nvidia-utils.  Had problems with the 390xx (the older version) running kitty. Have yet to see how this works for games.

# dotfiles

`git clone https://github.com/jam1015/dotfiles` and run the copying script from there.

# install i3/xorg

```
sudo pacman -S i3 xorg
sudo pacman -S i3 xorg-xinit
```

add `exec i3` to end of `~.xinitrc`.

# kitty
install it. see about drivers above.


# Change to zsh

```
sudo chsh -s /bin/zsh root
sudo chsh -s /bin/zsh jordan
```
`pacman -S gnustep-base` to install `defaults` command I use to set keyboard rate.


# Yay

```
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

# NVM

```
yay -S  nvm
echo 'source /user/share/nvm/init-nvm.sh' > ~/.zshrc
source ~/.zshrc
nvm install node
```

# tmux

```
sudo pacman -S tmux
yay -S tmux-plugin-manager #or install directly from github
sudo pacman -S xdg-utils
tmux source ~/.tmux.conf #to reload
```
then append

```
#run -b '~/.tmux/plugins/tpm/tpm' #if installed directly from github
run '/usr/share/tmux-plugin-manager/tpm' #if installed using yay
```
to `~/.tmux.conf`

`prefix + I` to install plugins (that is a capital i).

# neovim + packer

```
sudo pacman -S neovim
sudo pacman -S xclip
yay -S nvim-packer-git
```

In `nvim` run

```
PackerSync
TSUpdateSync
```
or possibly

```
TSUpdate
```


# set time
`timedatectl list-timezones`
`timedatectl set-timezone America/New_York`
`systemctl enable systemd-timesyncd`
`timedatectl set-ntp true`
`timedatectl set-local-rtc 1`



# Anaconda

```
pacman -Sy libxau libxi libxss libxtst libxcursor libxcomposite libxdamage libxfixes libxrandr libxrender mesa-libgl  alsa-lib libglvnd
yay -S anaconda
source /opt/anaconda/bin/activate root
conda update conda
conda update anaconda
conda deactivate
```

# Transfer Physical USB contents

```
fdisk -l
sudo mount /dev/sdb /mnt/
sudo rsync --archive --progress /mnt/Documents ~/

```

# set i3 prefix/ date

```
sudo timedatectl set-timezone 'America/New_York'
sudo timedatectl set-ntp true
```
in mod file

```
set $mod Mod4
```

make sure to use `$mod` instead of and explicit writing of `Mod{number}` in the rest of the file.

# screen saver
in .xinitrc put 
`xset dpms 1200 1205 1210` to turn off screen  after a bit.

# arandr
use it and source the resulting commands in `xinitrc`

# setup github
(easier with kitty+clipboard)
```
git clone https://gitlab.com/jam1015/ght
yay -S github-cli
gh auth login
```
select `HTTPS` and say `Y`

# install 
`rofi`
`firefox-developer-edition`
