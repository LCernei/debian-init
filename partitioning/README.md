# Partitionig, OS installation and bootloaders

![Final layout](https://i.imgur.com/OnuBd3k.png)
(Some size values might differ)

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
(The reserved and recovery partitions area created automatically)

## Installing rEFInd

Boot from a rEFInd USB stick.\
Select the linux os (probably has the Ubuntu logo).\
Open terminal, and open a root shell: `sudo su -`.\
Go to the efi partition:`cd /boot/efi`.\
Backup the Windows bootloader: `tar cvf win10.tar EFI/Windows && mv win10.tar ~/`.\
Clean the residual bootloaders: `rm -rf *`.\
Install the rEFInd bootloader: `apt install refind`.\
Restore the Windows bootloader: `tar xvf ~/win10.tar -C EFI/`.\
Reboot.

Now we have a dual-boot system (Windows 10 and Pop!_OS) managed by the rEFInd bootloader.

## Unrelated tips

I use Windows 10 WSL (debian). To confire IDEs to use the WSL git comand:\
Create a `git.cmd` file in a separate location (`C:\Users\user\source\wslWrapper`).\
Add this location to system Path.\

Write the following lines inside `git.cmd`:
```bat
@echo off
%WINDIR%\System32\bash.exe -c "git %*"
```

When the IDE looks for `git.exe` indicate `C:\Users\user\source\wslWrapper\git.cmd`.

[Tip Source](https://intellij-support.jetbrains.com/hc/en-us/community/posts/115000176290/comments/360000277759)