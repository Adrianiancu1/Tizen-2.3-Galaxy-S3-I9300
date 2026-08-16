# **Tizen 2.3 on Samsung Galaxy S3 I9300**

There are 2 important Samsung prototypes/reference devices

  The RD-210, which has the Galaxy S2 HD LTE as a base, codenamed GT-I9500, even though the GT-I9500 is the Galaxy S4 Exynos.
  
  The RD-PQ, which has the Galaxy S3 as a base, so basically this is your target, this device helps you get Tizen on the Galaxy S3 I9300.


Anyways, now you know whats inside the devices, so lets get to the main point of the project.

### WARNING(1): I AM NOT RESPONSIBLE FOR BRICKED DEVICES!!! DO THIS AT YOUR OWN RISK AND (maybe) COST!!!

### WARNING(2): AFTER INSTALLING TIZEN THERE ARE NO APPS, YOU WILL SEE ONLY THE SETTINGS APP, THE APPS THEMSELVES ARE LOST MEDIA!!!

### WARNING(3): YOU NEED UBUNTU/LINUX MINT (DEBIAN) FOR THIS PROJECT (RECOMMENDED: 14.04 TO 20.04)


## **LINKS:**

I have prepared a google drive link so you dont have to find the files yourself:

https://drive.google.com/file/d/19JF-j9O4_j14fGTtIC_hPtPI0Qg5KU7O/view?usp=sharing

https://drive.google.com/file/d/1TVGEgND-660y_pZMiM4vyHQUtIClAuLp/view?usp=drive_link (2nd link is because i forgot dump.bin which is needed for bootloader)

https://dl.twrp.me/i9300/twrp-2.4.0.0-i9300.tar.html

https://download.tizen.org/sdk/Installer/tizen-studio_6.1/

## **TOOLS:**

The twrp recovery is recommended to be flashed with heimdall

The bootloader is recommended to be flashed with adb

You will also need to install LThor tool.

If you ever want to mess with the device, you will need Tizen Studio (link from above)


## **EXPLANATION OF THE FILES:**

  FULLsizeTemplatev1.bin - This is a Windows tizen flasher tool that youre gonna have to make yourself since its a template
  
  tizen-2.3-mobile_20150311.3_mobile_target.tar.gz - The Tizen 2.3 firmware
  
  tizen-2.3-mobile_20150311.3_mobile_boot.tar.gz - you dont really need this, but its basically the bootloader
  
  i9300_emmc_toolbox.zip - Toolbox just in case you brick your device, or you want to go back to Android (I CAN NOT CONFIRM IF GOING BACK TO ANDROID WORKS) https://avalls.dev/i9300-EMMC-GUIDE/Explanation (link from another dev)
  
  revert_2.2.1_bootloader.tar.gz - Bootloader/pit revert for Tizen 2.2.1 (LOST MEDIA)
  
  migrate_2.3_bootloader.tar.gz - Bootloader/pit migration for Tizen 2.3 (you will need this AFTER you flash the bootloader onto the device)
  
  u-boot-mmc.zip - The bootloader thats for Tizen, you dont need this for S3
  
  ThorV2.rar - This basically tests your device if Windows can actually do a handshake between the device in Thor Mode (Download Mode)
  
  s-boot-mmc.zip - Actual bootloader needed, includes uboot, you will flash this with ADB
  
  dump.bin - Forgot but i think its basically params.bin, you also need this like s-boot-mmc.zip

## **BOOTLOADER FLASHING!!**

Download TWRP from the link, then rename the img to recovery.img

If you have heimdall, boot to Download Mode and run:

`heimdall flash --RECOVERY ./recovery.img --no-reboot`

after that, go in recovery mode, stay on the home screen and onto your Linux PC run:


`adb devices` (check this)

`adb shell`

`adb push dump.bin /external_sd`

`adb push s-boot-mmc.bin /external_sd`

`dd if=/external_sd/dump.bin of=/dev/block/mmcblk0`

`echo 0 > /sys/block/mmcblk0boot0/force_ro`

`dd if=/external_sd/s-boot-mmc.bin of=/dev/block/mmcblk0boot0`

`poweroff`


WARNING: if external_sd doesnt work, try replacing that with sdcard, or maybe try to actually put an sd card inside, (i have tried without sd card and it worked)  

YOU SHOULD NOW HAVE THE TIZEN BOOTLOADER ON YOUR PHONE  !!!!

If you have bricked the phone,

`sudo apt update`

`sudo apt install python3`

`sudo apt install python3 python3-libusb1 libusb-1.0-0 gcc-arm-none-eabi binutils-arm-none-eabi build-essential`

`sudo apt update`

And then go through the toolkit steps, i gave a link above btw.


Now, we have to install LThor onto our Linux PC and flash the firmware and newer bootloader for tizen 2.3!


## **HOW TO INSTALL LTHOR:**

`sudo apt update`

`sudo apt install libarchive*`

`sudo apt install libarchive13:i386 libarchive-tools:i386`

`sudo apt update`

`sudo apt purge modemmanager`

`sudo vi /etc/apt/sources.list`

(put at bottom)

`deb [trusted=yes] http://download.tizen.org/tools/archive/16.01/Ubuntu_14.04/ ./`

`sudo apt-get update`

`sudo apt-get install lthor`

OPTIONAL:

`sudo apt-get install gbs mic`

`sudo apt-get install device-tree-compiler`

`sudo apt-get install sdb`





modemmanager isnt recommended for flashing, i wouldnt recommend it since the beginning, you can add it back after doing everything  


Well done! You should now have lthor installed.


## **FLASHING THE NEW BOOTLOADER AND TIZEN 2.3**


Boot your phone into Download/Thor Mode (Power+VolDown+Home)

run

lsusb

after that, you should see your phone as I9100 in Download Mode

Now, flash it with the migrate_2.3_bootloader.tar package

DISCONNECT THE PHONE, RUN THE COMMAND AND THEN CONNECT IT!

`lthor migrate_2.3_bootloader.tar`

reboot device again to download mode

do the same thing, but:

`lthor tizen-2.3-mobile_20150311.3_mobile_target.tar`

reboot phone without pressing any buttons


If the phone isnt being detected by lthor, or its simply spamming "USB Port not detected!" or something like that, do the things below:


Run lsusb, look at the phones ID (example: 04e8:685d)

with our id, we will run:

Not sure if these commands are necessary but try:

`echo "1-1.4:1.1" | sudo tee /sys/bus/usb/drivers/cdc_acm/unbind `

`dmesg | tail -40`

`sudo dmesg -C`

This command is important though:

`echo "04e8 685d" | sudo tee /sys/bus/usb/drivers/cdc_acm/new_id`

And now try again.      


# DONE, YOU SHOULD NOW HAVE TIZEN ON YOUR SAMSUNG GALAXY S3 I9300!                                                                                         
