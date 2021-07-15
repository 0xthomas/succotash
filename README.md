# Ubuntu Server on Raspberry Pi 4 from SSD

This tutorial explains the steps needed to boot Ubuntu Server from an SSD connected to a Raspberry Pi 4. An SD card is needed during the installation proces, but when finished the SD card can be removed.

First the operating system for Raspberry Pi needs to be downloaded and flashed to an SD card. Go for the latest version of Raspberry Pi OS Lite (https://www.raspberrypi.org/software/operating-systems/). If you want you can sha256sum the file and compare it to the hash on the website:

```
echo "<sha256 hash to compare against> <file-to-hash>" | sha256sum -c
Output should be: **OK**
```

Disks
Restore Disk Image
