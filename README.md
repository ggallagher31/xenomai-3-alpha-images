Xenomai-3 Reference Images:
Ipipe Version 4.14
Xenomai-3 Stable 3.0.x

These images are based on the latest 4.14 ipipe development branch and the latest Xenomai-3 branch stable-3.0.x 

Supported Boards:
------------------------
Raspberry Pi2 

These images are intended to get a developer up and running on Xenomai with as quickly as possible.  You will not have to build anything from source to get these images to run.  

Getting Started:
------------------------
Creating a bootable sdcard:
Download the rootfs and board specific files from here https://drive.google.com/open?id=1xH931BOxpV6YGOTx90peSs3ODvtDJJUt

There are two files, one is an rootfs for the armv7 arch, this rootfs is based on Ubuntu 16.04 the second is the boot files, kernel and drivers.

Extract the rootfs, do this as sudo:
        sudo tar xpvzf xenomai-3-rootfs-armv7.tar.gz
        
This should create a directory called ubuntu_16_04_armhf_rootfs

Raspberry PI2 Instructions:
-----------------------------
Extract the kernel, boot files and kernel modules:
        sudo tar xpvzf xenomai-3-rpi_boot_modules.tar.gz

We need to have an sdcard with two partitions, one will hold the rootfs and the other will hold the
boot files (kernel, bootloader, devicetree etc).

Preparing sdcard:
------------------------------
Create two partitions, one will be FAT32, one will be EXT-4
Partition sizes can vary; your FAT32 parition can be about 64MB and the rest can be EXT-4

Creating the partitions is a excercise for the reader, but the easiest way to do this is to use a tool like gparted.

Copy Rootfs and boot files:
--------------------------------
Copy the boot files to the boot partition:
 rsync -aAxv ./rpi2_boot_modules/boot/* <path to sdcard>/boot/

Copy the rootfs to the sdcard
sudo rsync -aAxv ./rootfs/* <path to sdcard>/rootfs/

Copy the kernel modules over to the sdcard
  sudo rsync -aAxv ./rpi2_boot_moduels/lib/modules/* <path_to_sdcard>/lib/modules/

At this point we should have a working image copied over to our sdcard, we can now go ahead and boot it and try to
login.

Login for the system is :
User : xeno
passwd : xeno

Setting up Tools:
--------------------------------
We will also need a cross compiler.  I recommend using the Linaro arm-none-linux-eabihf- compiler. 
environment. https://releases.linaro.org/components/toolchain/binaries/latest-5/arm-linux-gnueabihf/
Uncompress the toolchain and install it somewhere on your host system.  

Make sure you add the path to the cross-compiler to your PATH variable so it can be found when we use the makefile

Creatng a Test Application:
--------------------------------
Before we can build the app make sure that you switch the Makefile to use the correct path to the root filesystem.  This
path should be the path to where you stored the root filesystem on the host before it was copied over to the sdcard.

git clone https://github.com/ggallagher31/simple_rt_app.git

Build the simple rt app
cd simple_rt_app
make

You can copy over the executable to the sdcard and run the very simple rt application.

