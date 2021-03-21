# Debian configuration

I started with a minimal server installation (no DE, no system-tools, etc.).\
First time the system boots into text mode.

## (Optional) Remove grub
```
su -
apt purge grub*
rm -r /boot/grub
rm -r /boot/efi/EFI/debian
```

## Debian Sid
```
apt install vim
vim /etc/apt/sources.list
```
Replace the content of `/etc/apt/sources.list` with the lines below (non-free contrib are optional).
```
deb http://deb.debian.org/debian/ sid main non-free contrib
deb-src http://deb.debian.org/debian/ sid main non-free contrib
```
Save the file and apply the new changes:
```
sudo apt update
sudo apt full-upgrade
```

## Install Ansible
```
apt install git sudo ansible
usermod -aG sudo lc
reboot
cd Documents
git clone git@github.com:LCernei/os-tweaks.git
cd os-tweaks/ansible
```
Run the ansible playbook:
```
ansible-playbook site.yaml -i hosts -K
```
Reboot. (Optional: restore the dotfiles)

## Nvidia with Optimus-switch
This works better than system76-power. Is simpler and more flexible.\
[optimus-switch-gdm](https://github.com/dglt1/optimus-switch-gdm) 

## RTL8822BE wifi
Install `firmware-linux` and `firmware-realtek` (may not be necessary in the future). 

```
echo "blacklist ideapad_laptop" | sudo tee /etc/modprobe.d/blacklist-ideapad.conf
echo "options r8822be aspm=0" | sudo tee /etc/modprobe.d/r8822be.conf
sudo reboot
```

## Steam `libGL.so.1` error
```
sudo apt install libgl1-nvidia-glvnd-glx:i386
```
