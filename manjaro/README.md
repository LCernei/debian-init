# Manjaro configuration

## system76-power

Install https://aur.archlinux.org/packages/system76-power/ and its dependencies.\
Create a service file:
```
sudo vim /etc/systemd/system/system76power.service
```
Copy the configuration from https://github.com/pop-os/system76-power/blob/master/debian/system76-power.service

### Switching graphichs

```
sudo systemctl start system76power
sudo system76-power graphics [nvidia|integrated]
sudo systemctl stop system76power
sudo reboot
```

## RTL8822BE wifi

```
echo "blacklist ideapad_laptop" | sudo tee /etc/modprobe.d/blacklist-ideapad.conf
echo "options r8822be aspm=0" | sudo tee /etc/modprobe.d/r8822be.conf
sudo reboot
```

## Useful commands

```
sudo pacman -S package	        # install package
sudo pacman -Syyu		        # update & upgrade
pamac upgrade -a		        # upgrade including AUR
pacman -Ss package		        # search package
pacman -Qs package		        # search installed package
pacman -Si package		        # display info about package
sudo pacman -Rs package	        # remove package ('s' for dependencies)
sudo pacman -Qdt		        # list orphans (unused packages)
sudo pacman -Rs $(pacman -Qdtq)	# remove orphans

# Install from AUR:
# Copy the Git Clone URL
git clone url
cd clodedPackage
makepkg -s
sudo pacman -U clonedPackage.tar.xz

```
