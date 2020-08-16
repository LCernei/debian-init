# Debian configuration

## Debian Sid
Edit `/etc/apt/sources.list` (non-free contrib are optional).

```
deb http://deb.debian.org/debian/ sid main non-free contrib
deb-src http://deb.debian.org/debian/ sid main non-free contrib
```
```
sudo apt update
sudo apt full-upgrade
```
## Nvidia graphics with system76-power
Install an System76 recommended kernel (compatible with the graphics driver). Check the currently installed version with `uname -r`.
```
sudo apt install linux-image-5.4.0-7634-generic
```
Boot from the newly installed kernel and remove the old one (optional).
```
sudo apt remove --purge linux-image-5.7.0-1-amd64
```
Add the System76 repository to the apt sources.
```
sudo apt-add-repository -ys ppa:system76-dev/stable
sudo apt update
```
There may appear an gpg error "... public key is not available: NO_PUBKEY ...". It may be fixed using the \<KEY\> in the error.
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <KEY>
```
Download `python3-xkit` from [pkgs.org/download/python3-xkit](https://pkgs.org/download/python3-xkit) and install using apt.
```
sudo apt install ./python3-xkit_0.5.0ubuntu4_all.deb
```
Install the System76 related tools and drivers
```
sudo apt install system76-driver-nvidia system76-power
```
Switch to Nvidia graphics and reboot. You can switch back with `system76-power graphics internal`.
```
system76-power graphics nvidia
sudo reboot
```
After the reboot (for Nvidia), only the external monitors will work.\
TO DO: Find a solution for blank internal display when using Nvidia.

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
