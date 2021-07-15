# Ubuntu Server on Raspberry Pi 4 from SSD

This tutorial explains the steps needed to boot Ubuntu Server from an SSD connected to a Raspberry Pi 4. An SD card is needed during the installation proces, but when finished the SD card can be removed.

First the operating system for Raspberry Pi needs to be downloaded and flashed to an SD card. Go for the latest version of [Raspberry Pi OS Lite](https://www.raspberrypi.org/software/operating-systems/). Additionaly you can sha256sum the file and compare it to the hash on the website:

```bash
echo "<sha256-hash-to-compare-against> <RaspberryPiOs.zip>" | sha256sum -c
Output should be: OK
```

Unzip the downloaded file and write the image to the SD card with your favorite image writer. Alternatively you can use the command line:

```bash
gunzip -c <RaspberryPiOs.zip> | dd of=/dev/sdcard
```

