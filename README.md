# Ubuntu Server on Raspberry Pi 4 from SSD

This tutorial explains the steps needed to boot Ubuntu Server from an SSD connected to a Raspberry Pi 4. An SD card is needed during the installation proces, but when finished the SD card can be removed.

## SD card
First the operating system for Raspberry Pi needs to be downloaded and flashed to an SD card. Go for the latest version of [Raspberry Pi OS Lite](https://www.raspberrypi.org/software/operating-systems/). Additionaly you can sha256sum the file and compare it to the hash on the website:

```bash
echo <sha256-hash-to-compare-against> <RaspberryPiOs.zip> | sha256sum -c
Output should be: OK
```

Unzip the downloaded file and write the image to the SD card with your favorite image writer. Alternatively you can use the command line:

```bash
gunzip -c <RaspberryPiOs.zip> | dd of=/dev/sdcard
```

To avoid the use of an additional screen for the Raspberry Pi, we will enable SSH before we put the SD card back in the Raspberry Pi. To do this we need to create a file called "ssh" on the boot-partition of the SD card:

```bash
touch /path-to-sd-card/boot/ssh
```

## SSD
The next step is preparing the SSD with Ubuntu Server. The steps are roughly the same but with a different device and a different image. The image is available for download [here](https://ubuntu.com/download/raspberry-pi). It is recommended to go with the 64b-it LTS version. To check the file integrity you can check the hash from the website with the hash from the downloaded file:

```bash
echo <sha256-hash-to-compare-against> <UbuntuServer.img.xz> | sha256sum -c
Output should be: OK
```

Extract the downloaded file and write the image to the SSD:

```bash
unxz -c <UbuntuServer.img.xz> | dd of=/dev/ssd
```

Ubuntu Server has SSH enabled by default which means we don't need to create a file called ssh. Next we can put the SD card in the Pi and make sure the Pi is connected with an ethernet cable. Do not connect the SSD yet as the Pi's default bootorder is first USB, then SD. Power up the Pi.

## Prepare Boot from SSD

The Pi is now powered up, booting from the SD card with an active internet connection through the ethernet cable. After establishing an SSH connection with the Pi, we will update and reboot the Raspberry Pi:

```bash
sudo apt update && sudo apt full-upgrade -y
sudo reboot now
```

Next we need to check if the Pi's bootloader firmware is up to date:

```bash
sudo rpi-eeprom-update -a
```

If the bootloader wasn't up-to-date then a reboot is required. The next step is preparing the SSD with Ubuntu. Connect the SSD to the Pi and mount the partitions on the mountpoints as depicted below:

```bash
lsblk
```

The SD card is named something like "mmcblk0". We need to note down the name of the SSD which is the other device listed after the `lsblk`-command. We need to make mountpoints for both partitions and then mount both partitions:

```bash
sudo mkdir /mnt/boot
sudo mkdir /mnt/writable
sudo mount /dev/sda1 /mnt/boot
sudo mount /dev/sda2 /mnt/writable
```

With the `lsblk`-command you can check if the SSD is correctly mounted. The SSD with Ubuntu will be made bootable with the script provided by GitHub-user TheRemote (for availability purposes the script is also hosted on this GitHub repository, all the credits of course go to TheRemote):

```bash
sudo apt install git
sudo curl https://raw.githubusercontent.com/TheRemote/Ubuntu-Server-raspi4-unofficial/master/BootFix.sh | sudo bash
```

Now unmount the partitions and shutdown the Pi:

```bash
sudo umount /mnt/boot
sudo umount /mnt/writable
```

Remove the SD card and power up the Pi.

## Configure Ubuntu Server


