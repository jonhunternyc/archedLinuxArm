#!/bin/bash

function start {
  echo "Unmounting /dev/$sd...";
	umount /dev/${sd}*;

	echo "Creating disk label for GPT";
	partx -t gpt -u /dev/$sd;
	
	echo "Creating partitions"; 
	cgpt create -z /dev/$sd;
	cgpt create /dev/$sd;
	cgpt add -i 1 -t kernel -b 8192 -s 32768 -l U-Boot -S 1 -T 5 -P 10 /dev/$sd;
	cgpt add -i 2 -t data -b 40960 -s 32768 -l Kernel /dev/$sd;
	cgpt add -i 12 -t data -b 73728 -s 32768 -l Script /dev/$sd;
	
	cgpt show /dev/$sd;
	echo "How large do you want your rootfs partition? (61337599)";
	read size;
	cgpt add -i 3 -t data -b 106496 -s `expr 61337599 - 409060` -l Root /dev/$sd;
	
	echo "Refreshing disk partition list";
	partx -l /dev/$sd;
	
	echo "Formating new partitions";
	mkfs.ext2 /dev/${sd}p2;
	mkfs.ext4 /dev/${sd}p3;
	mkfs.vfat -F 16 /dev/${sd}p12;
	
	echo "Moving to tmp folder";	
	cd /tmp;
	
	echo "Downloading latest version of Arch chromebook...";
	curl -LO http://os.archlinuxarm.org/os/ArchLinuxARM-armv7-chromebook-latest.tar.gz;
	
	mkdir root;
	echo "Mounting root partition in tmp to extract Arch";
	mount /dev/${sd}p3 root;
	tar -xvzf http://os.archlinuxarm.org/os/ArchLinuxARM-armv7-chromebook-latest.tar.gz -C root;
	
	mkdir mnt;
	mount /dev/${sd}p2 mnt;
	cp root/boot/vmlinux.uimg mnt;
	umount mnt;
	
	mount /dev/${sd}p12 mnt;
	mkdir mnt/u-boot;
	curl -LO http://archlinuxarm.org/os/exynos/boot.scr.uimg;
	cp boot.scr.uimg mnt/u-boot;
	umount mnt;

	curl -LO http://commondatastorage.googleapis.com/chromeos-localmirror/distfiles/nv_uboot-snow.kpart.bz2 | bunzip2 > nv_uboot-snow.kpart;
	dd if=nv_uboot-snow.kpart of=/dev/${sd}p1;

	umount root;
	sync;

	echo "Done";
}

echo "Drive label?"
read sd;
echo "Target drive is=/dev/$sd";
echo "Is this correct (Yes, No)?";
read ans;

if [[ "$ans" == "y" || "$ans" == "yes" || "$ans" == "Yes" ]];
then
	start;
else
	if [[ "$ans" == "n" || "$ans" == "no" || "$ans" == "No" ]];
	then
		echo "Retrying...";
		./ArchSD-Install.sh;
	else
		echo "Please answer (Yes or No)";
		echo "Retrying...";
		./ArchSD-Install.sh;
	fi
fi
