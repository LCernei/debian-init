# Partitionig, OS installation and bootloaders
## Partition layout example
![Final layout](https://i.imgur.com/OnuBd3k.png)

## General approach
I found it easier to install the linux system before Windows. Microsoft automatically creates some partitions - this may result in unexpected modifications to the planned layout. That's why I initially create the linux partitions and leave a block of free spcae. This way, Windows is forced to fit everything in that block.

## Installing Pop!_OS
Boot from a pop_os USB stick.\
Proceed with the installation till the "Try or Install" step.\
Select "Custom (Advanced)".\
Select the drive and click "Customize Partitions...".\
It starts the GParted partition manager.
Create a new partition table (it wipes the entire disk).\
Create a 1 GB partition for efi (boot).\
Leave 200 GB for Windows (free space for now).\
Create 200 GB ext4 partition for Linux.\
Create a 10 GB swap partition at the end of the disk.\
Edit -> Apply All Operations; then close GParted.\
Select the first partition, activate the "Use partition" toggle, "Use as:" Boot(/boot/efi), the second - "Use as:" Root(/), the third - "Use as:" Swap
Click "Erase and Install".\
Install Pop!_OS as usual.

## Installing Windows10
Boot from a win10 USB stick.\
Proceed with the installation till the "Which type of installation do you want?" step.\
Select "Custom: Install Windows only (advanced)".\
Select the empty 200 GB space allocated before (between Boot and Linux).\
Click next and install Windows 10 as usual.\
(The reserved and recovery partitions are created automatically)

## Installing rEFInd
Boot into the linux system: either using a rEFInd USB stick or by initially editing EFI boot entries in Windows (using EasyUEFI by Hasleo)\
Open terminal, and open a root shell: `sudo su -`.\
Go to the efi partition:`cd /boot/efi`.\
Backup the Windows bootloader: `tar cvf win10.tar EFI/Windows && mv win10.tar ~/`.\
Clean the residual bootloaders: `rm -rf *`.\
Install the rEFInd bootloader: `apt install refind`.\
Restore the Windows bootloader: `tar xvf ~/win10.tar -C EFI/`.\
Reboot.

Now we have a dual-boot system (Windows 10 and Pop!_OS) managed by the rEFInd bootloader.

To set a custom icon for each OS, use
```bash
sudo tune2fs -L myname /dev/mypartition
```
and provide a `os_myname.png` file inside `/EFI/refind/icons`.\
(Replace `myname` and `mypartition` with the coresponding label and partition. Only works for ext file systems.) \
Another approach for icons:
> To customize the icons, simply copy the icon that you want to use, to root of system partition. \
> Rename it to .VolumeIcon.png. Refind supports PNG, JPG, BMP, and ICNF icons.

Source: [TeejeeTech](https://teejeetech.in/2020/09/05/linux-multi-boot-with-refind/)

## Installing GRUB2 (aternative to rEFInd)
`apt install grub-efi-amd64`\
`grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB` optionally you can add `--removable`\
`grub-mkconfig`
