# archedLinuxArm

This interactive bash script will install Arch Linux ARM onto your SD card or USB drive from ChromeOS. This will allow you to boot directly onto the SD card and avoid ChromeOS or ChrUbuntu all together.
Installation guide for XE303C12
We need access to root for this script to work, we can get access by enabling developer mode.
The latest image of Arch Linux ARM seems to have broken drivers for wifi. Replace wget link on line 34 with http://rollback.archlinuxarm.org/os/{year}/{month}/{day}/ArchLinuxARM-chromebook-latest.tar.gz for older working versions of Arch.

Enabling developer mode

Turn off device.

Hold ESC and Refresh, then tap the power button.

Confirm switch to developer mode.

Press CTRL-D to boot when device restarts.

We cannot boot onto the our USB or SD card until we allow it via root.

Enabling USB/SD booting

After booting into ChromeOS in developer mode, tap CTRL-ALT-F2 to enter console.

Login using default user: chronos

Login to root using command: sudo su

Enter this command: crossystem dev_boot_usb=1 dev_boot_signed_only=0

Reboot device.

Installing Arch

Login as root in ChromeOS, then enter the following commands:

cd /tmp
curl -O https://raw.githubusercontent.com/jonhunternyc/archedLinuxArm/main/bash_install.sh

After it finishes, reboot your device and tap CTRL-U to boot onto your USB or SD card when you are greeted with the bootloader.

Based on this guide: http://archlinuxarm.org/platforms/armv7/samsung/samsung-chromebook
