---
Title: Linux config
Author: Jordan Mandel
---

# Graphics Drivers

consider nvidia-390xx-utils vs nvidia-utils.  Had problems with the 390xx (the older version) running kitty. Have yet to see how this works for games.

# dotfiles

`git clone https://github.com/jam1015/dotfiles` and run the copying script from there.

# Change to zsh

```
sudo chsh -s /bin/zsh root
sudo chsh -s /bin/zsh jordan
```
`pacman -S gnustep-base` to install `defaults` command I use to set keyboard rate.

# Network Configuration
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

log onto network with `nmtui`

# Yay

```
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yaya
makepkg -si
```

# NVM

```
yay -S  nvm
echo 'source /user/share/nvm/init-nvm.sh' > ~/.zshrc
source ~/.zshrc
```

# tmux

```
sudo pacman -S tmux
yay -S tmux-plugin-manager #or install directly from github
```
then append

```
#run -b '~/.tmux/plugins/tpm/tpm' #if installed directly from github
run '/usr/share/tmux-plugin-manager/tpm' #if installed using yay
```

`prefix + I` to install plugins (that is a capital i).

# neovim + packer

`sudo pacman -S neovim`
`sudo pacman -S xclip`
`yay -S nvim-packer-git`


# kitty
install it. see about drivers above.

# screen saver
in .xinitrc put 
`xset dpms 1200 1205 1210` to turn off screen  after a bit.


# set time
`timedatectl set-local-rtc 1`

# setup github
```
yay -S github-cli
gh auth login
```
select `HTTPS` and say `Y`

# Anaconda

```
pacman -Sy libxau libxi libxss libxtst libxcursor libxcomposite libxdamage libxfixes libxrandr libxrender mesa-libgl  alsa-lib libglvnd
yay -S anaconda
source /opt/anaconda/bin/activate root
conda update conda
conda update andaconda
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
