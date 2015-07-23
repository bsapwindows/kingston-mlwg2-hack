Introduction
------------

Verifying installation of OpenWrt on a Kingston MLWG2 with factory firmware.

Tested with the `G2.zip` file provided by Larry D. Pinney on 2015-03-08.

## Prerequisites

1. One MLWG2
2. One FAT32-formatted SD-Card (or USB pendrive) with at least 10 MB free

### Optional (for serial console)

1. A USB to 3.3V TTL dongle
2. A terminal emulator to control the serial console - See [[TODO]]

### Optional (for TFTP server)

4. One Linux host configured as a TFTP server - See [[Installing TFTP Server on Ubuntu 14.04.1]]
5. One Ethernet cable to connect the MLWG2 to the TFTP server

## Preparation

Create a new test directory

```
$ mkdir -p test-G2
$ cd test-G2
```

Download file `G2.zip` from Google Drive, then unzip archive

```
$ unzip ~/Downloads/G2.zip
```

Inspect result

```
gmacario@ITM-GMACARIO-W7 /cygdrive/e/data/MYGIT/kingston-mlwg2-hack/test-G2
$ ls -la
total 22193
drwxr-xr-x+ 1 gmacario Domain Users       0 Mar 15 11:37 .
drwxr-xr-x+ 1 gmacario Domain Users       0 Mar 15 10:55 ..
-rw-r--r--  1 gmacario Domain Users      52 Mar  4 23:15 mlwG2_header.bin
-rw-r--r--  1 gmacario Domain Users 6504173 Sep 19 08:03 mlwG2_v2.0.0.6.bin
-rw-r--r--  1 gmacario Domain Users 6504121 Mar  7 21:15 mlwG2_v2.0.0.8.bin
-rw-r--r--  1 gmacario Domain Users 9699384 Mar  7 11:51 mlwG2_v2.0.0.9.bin
-rw-r--r--  1 gmacario Domain Users     851 Mar  7 21:20 README.txt

gmacario@ITM-GMACARIO-W7 /cygdrive/e/data/MYGIT/kingston-mlwg2-hack/test-G2
$
```

Verify file checksums

```
gmacario@ITM-GMACARIO-W7 /cygdrive/e/data/MYGIT/kingston-mlwg2-hack/test-G2
$ md5sum *
b91ad58be8b1f01c5922f95eb41785ec *mlwG2_header.bin
5746238fa47659154fb8db2eb02acf08 *mlwG2_v2.0.0.6.bin
7dcf5c29051d9bde3232632f8d312095 *mlwG2_v2.0.0.8.bin
ac3910d4cfefabbc232a490430d065cd *mlwG2_v2.0.0.9.bin
b65b6ac3489765fea0ccba493885cc73 *README.txt

gmacario@ITM-GMACARIO-W7 /cygdrive/e/data/MYGIT/kingston-mlwg2-hack/test-G2
$
```

### Optional: Connect a serial console

Connect the header of the USB-to-TTL dongle to the pins on the MLWG2

| MLWG2 pin |  Signal | Notes                     |
| ---------:| -------:| ------------------------- |
|         1 | +3.3Vdc | DO NOT CONNECT            |
|         2 |      TX | Connect to dongle pin "R" |
|         3 |      RX | Connect to dongle pin "T" |
|         4 |     GND | Connect to dongle pin "G" |

Plug the USB-to-TTL dongle into the PC that will run the terminal emulator.

Launch the terminal emulator to control the MLWG2 serial console
* Parameters: COMx:57600,8,n,1

### Optional: Connect to the TFTP server

Connect the Ethernet port in the MLWG2 to the TFTP server via the Ethernet cable

## Install OpenWrt from MLWG2 Factory Firmware

(2015-03-15: Tested with `G2.zip/mlwG2_v2.0.0.9.bin`)

Read and follow the instructions in the `README.txt` file.

Write file `mlwG2_v2.0.0.9.bin` on the root directory of the SD-Card.

Power up the MLWG2 and wait for the firmware update to complete.

FIXME: The kernel upgrade procedure failed (tried three times with no success)

```
...
INFO:Wrote 64 KB
INFO:Wrote 64 KB
INFO:Wrote 64 KB
INFO:Wrote 64 KB
INFO:Wrote 64 KB
nvram_upgrade: ERROR==>Write kernel failure
Upgrade /media/USB1/mlwG2_v2.0.0.9.bin(9472 KB) failed

The system is going down NOW!

Sending SIGTERM to all processes

Sending SIGKILL to all processes

Requesting system reboot
Restarting system.



U-Boot 1.1.3 (Dec 30 2013 - 10:32:24)


Board: Ralink APSoC DRAM:  64 MB

relocate_code Pointer at: 83fb4000

enable ephy clock...done. rf reg 29 = 5

SSC disabled.

******************************

Software System Reset Occurred

******************************

spi_wait_nsec: 29

spi device id: c2 20 18 c2 20 (2018c220)

find flash: MX25L12805D

raspi_read: from:30000 len:1000

raspi_read: from:30000 len:1000

============================================

Ralink UBoot Version: 4.1.1.0

--------------------------------------------

ASIC 7620_MP (Port5<->None)

DRAM component: 512 Mbits DDR, width 16

DRAM bus: 16 bit

Total memory: 64 MBytes

Flash component: SPI Flash

Date:Dec 30 2013  Time:10:32:24

============================================

icache: sets:512, ways:4, linesz:32 ,total:65536

dcache: sets:256, ways:4, linesz:32 ,total:32768


 ##### The CPU freq = 580 MHZ ####

 estimate memory size =64 Mbytes


Please choose the operation:

   1: Load system code to SDRAM via TFTP.

   2: Load system code then write to Flash via TFTP.

   3: Boot system code via Flash (default).

   4: Entr boot command line interface.

   7: Load Boot Loader code then write to Flash via Serial.

   9: Load Boot Loader code then write to Flash via TFTP.

 0

CFG_KERN_ADDR=bc050000

3: System Boot system code via Flash.

3: System Boot on bootstate=0.

## Booting image at bc050000 ...

raspi_read: from:50000 len:40

   Image Name:   Linux Kernel Image

   Image Type:   MIPS Linux Kernel Image (lzma compressed)

   Data Size:    6504057 Bytes =  6.2 MB

   Load Address: 80000000

   Entry Point:  8000c310

raspi_read: from:50040 len:633e79

   Verifying Checksum ... OK

   Uncompressing Kernel Image ... OK

No initrd

## Transferring control to Linux (at address 8000c310) ...

## Giving linux memsize in MB, 64


Starting kernel ...



LINUX started...

 THIS IS ASIC
Linux version 2.6.36+ (root@CVS2) (gcc version 3.4.2) #2 Thu Sep 18 10:05:08 CST 2014

 The CPU feqenuce set to 580 MHz
...
```

### 2015-03-16: Testing updated mlwG2_v2.0.1.3.bin file from Larry

```
gmacario@ITM-GMACARIO-W7 ~
$ md5sum ~/Downloads/mlwG2_v2.0.1.3.bin
7e03b452a8fb04dc1786cfcf104a98c5 */cygdrive/c/Users/gmacario/Downloads/mlwG2_v2.0.1.3.bin

gmacario@ITM-GMACARIO-W7 ~
$
```

Write file `mlwG2_v2.0.1.3.bin` to the root directory of a FAT32-formatted USB pendrive, then turn on MLWG2.

Result: kernel erased and device reset.
Next time got a kernel panic:

```
...
[   20.060000] jffs2: jffs2_scan_eraseblock(): Magic bitmask 0x1985 not found at 0x004f0024: 0x4117 instead
[   20.080000] jffs2: Further such events for this erase block will not be printed
[   20.220000] jffs2: Cowardly refusing to erase blocks on filesystem with no valid JFFS2 nodes
[   20.240000] jffs2: empty_blocks 83, bad_blocks 0, c->nr_blocks 146
[   20.250000] VFS: Cannot open root device "(null)" or unknown-block(31,4): error -5
[   20.270000] Please append a correct "root=" boot option; here are the available partitions:
[   20.280000] 1f00             192 mtdblock0  (driver?)
[   20.290000] 1f01              64 mtdblock1  (driver?)
[   20.300000] 1f02              64 mtdblock2  (driver?)
[   20.310000] 1f03           15744 mtdblock3  (driver?)
[   20.320000] 1f04            9392 mtdblock4  (driver?)
[   20.330000] 1f05           15872 mtdblock5  (driver?)
[   20.340000] 1f06             320 mtdblock6  (driver?)
[   20.350000] Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(31,4)
```

On subsequent reboot the device reverts back to Ralink firmware.

I tried the procedure a couple of times and always get the same result.

I also find a couple of suspicious messages here:

```
[    0.430000] Creating 6 MTD partitions on "spi32766.0":
[    0.440000] 0x000000000000-0x000000030000 : "u-boot"
[    0.450000] 0x000000030000-0x000000040000 : "u-boot-env"
[    0.470000] 0x000000040000-0x000000050000 : "factory"
[    0.480000] 0x000000050000-0x000000fb0000 : "firmware"
[    0.490000] 0x000000683eb9-0x000000fb0000 : "rootfs"
[    0.500000] mtd: partition "rootfs" must either start or end on erase block boundary or be smaller than an erase block -- forcing read-only
[    0.530000] mtd: device 4 (rootfs) set to be root filesystem
[    0.540000] mtdsplit: no squashfs found in "spi32766.0"
[    0.550000] 0x000000080000-0x000001030000 : "firmware"
[    0.560000] mtd: partition "firmware" extends beyond the end of device "spi32766.0" -- size truncated to 0xf80000
```

Serial console logfile: `20150316-0812-mlwg2.txt`

### 2015-03-16: Testing updated mlwG2_v2.0.1.4.bin file from Larry

```
gmacario@ITM-GMACARIO-W7 ~
$ md5sum ~/Downloads/mlwG2_v2.0.1.4.bin
00c09ec25857ff874b865c44e0ca6b6c */cygdrive/c/Users/gmacario/Downloads/mlwG2_v2.0.1.4.bin

gmacario@ITM-GMACARIO-W7 ~
$
```

Write file `mlwG2_v2.0.1.4.bin` to the root directory of a FAT32-formatted USB pendrive, then turn on MLWG2.

Result: kernel and rootfs written to the MLWG2 flash, then correctly reloaded
after the MLWG2 is reset.

Pressing "ENTER" on the serial console you get an interactive OpenWrt shell:

```



BusyBox v1.22.1 (2015-03-14 22:48:43 CDT) built-in shell (ash)
Enter 'help' for a list of built-in commands.

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 BARRIER BREAKER (Barrier Breaker, r44697)
 -----------------------------------------------------
  * 1/2 oz Galliano         Pour all ingredients into
  * 4 oz cold Coffee        an irish coffee mug filled
  * 1 1/2 oz Dark Rum       with crushed ice. Stir.
  * 2 tsp. Creme de Cacao
 -----------------------------------------------------
root@OpenWrt:/#
```

Verify kernel version:

```
root@OpenWrt:/# uname -a
Linux OpenWrt 3.10.49 #1 Mon Mar 16 05:41:04 CDT 2015 mips GNU/Linux
root@OpenWrt:/#
```

Inspect mounted filesystems:

```
root@OpenWrt:/# df -h
Filesystem                Size      Used Available Use% Mounted on
rootfs                    4.4M    276.0K      4.2M   6% /
/dev/root                 2.8M      2.8M         0 100% /rom
tmpfs                    30.2M     64.0K     30.1M   0% /tmp
tmpfs                    30.2M     44.0K     30.2M   0% /tmp/root
tmpfs                   512.0K         0    512.0K   0% /dev
/dev/mtdblock5            4.4M    276.0K      4.2M   6% /overlay
overlayfs:/overlay        4.4M    276.0K      4.2M   6% /
root@OpenWrt:/#
```

Check networking (notice that I have no Ethernet cables plugged)

```
root@OpenWrt:/# ifconfig -a
br-lan    Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet addr:192.168.1.1  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fd40:986:8cbe::1/60 Scope:Global
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:27 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:2962 (2.8 KiB)

eth0      Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:20 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:3357 (3.2 KiB)
          Interrupt:5

eth0.1    Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:15 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 B)  TX bytes:1766 (1.7 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:2258 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2258 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:153504 (149.9 KiB)  TX bytes:153504 (149.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:14 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:1720 (1.6 KiB)

root@OpenWrt:/#
```

Force reboot

```
# sync
# sync
# halt
```

Notice that the Ralink factory firmware is loaded again.

TODO: Perform flashing of OpenWrt from LuCI before rebooting

Serial console logfile: `20150316-1245-mlwg2.txt`

### Testing files from Larry, 2015-03-18 02:32

```
gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-0232-ldpinney_new_G2_firmware_RFC
$ md5sum *
89927f8e461c54840e925b23c12581e0 *20150318-mail_ldpinney.pdf
80bd26aa2146bbc9aad8c1f04dbd1a00 *mlwG2_v2.0.1.6.bin
9d7b9dcf5e55c96b76f39d110cd8fb37 *openwrt-ramips-mt7620n-mlwg2-squashfs-sysupgrade.bin
69a4adad18f0b27ffe8e62dbef6c6ced *README.txt

gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-0232-ldpinney_new_G2_firmware_RFC
$
```

Copy file `mlwG2_v2.0.1.6.bin` to the root directory of a FAT32-formatted USB Pendrive

Press the "Power" button for 3 seconds, the blue LED should start blinking

TODO

### Testing files from Larry, 2015-03-18 22:02

```
gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-2202-ldpinney_new_G2_firmware_RFC
$ md5sum *.bin
4beaf60e6eae7e19481bc1b1ac6be68b *mlwG2_v2.A.0.0.bin
cf59fd0af3fcf1fac470c5adecb9a853 *mlwG2_v2.A.B.0.bin

gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-2202-ldpinney_new_G2_firmware_RFC
$
```

With the factory firmware running on the MLWG2, inspect the contents of the `/sbin/upgrade.sh` script

```
BusyBox v1.12.1 (2014-09-18 09:46:08 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

# cat /sbin/upgrade.sh
#! /bin/sh

#FW_buf=`ls /media/*/Kingston_widrive_v*.*.*.bin`
FW_buf=`ls /media/*/mlw5200fw_v*.*.*.bin`
nvram upgrade $FW_buf

FW_buf2=`ls /media/*/mlwG2_v*.*.*.bin`
nvram upgrade $FW_buf2

exit 0

#
```

From the host, copy the first file (without the trailing `.0` in the filename) on the root direcotry of a FAT32-formatted USB Pendrive

```
gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-2202-ldpinney_new_G2_firmware_RFC
$ cp mlwG2_v2.A.0.0.bin /cygdrive/h/mlwG2_v2.A.0.bin

gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-2202-ldpinney_new_G2_firmware_RFC
$ md5sum /cygdrive/h/mlwG2_v2.A.0.bin
4beaf60e6eae7e19481bc1b1ac6be68b */cygdrive/h/mlwG2_v2.A.0.bin

gmacario@ITM-GMACARIO-W7 /cygdrive/e/temp/OpenWrt_Kingston_MLWG2/20150318-2202-ldpinney_new_G2_firmware_RFC
$
```

Power up MLWG2 and keep the power button pressed for three seconds.
The "Internet" blue LED should turn on, then start blinking for one minute.

The flashing process should not take more than 5 minutes, then the MLWG2 should reboot with the OpenWrt (minimalistic) firmware.

FIXME: Got "Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(31,4)"

If the MLWG2 is reset the factory Ralink firmware is loaded again.

See boot log: TODO




## Restore MLWG2 Factory Firmware from OpenWrt

Login to LuCI, then
System > Backup / Flash Firmware

In section "Flash new firmware image"

* Keep settings: No
* Image: Choose File `mlwG2_v2.0.0.8.bin`
* then select "Flash image..."

Verify that the printed checksum is equal to the local MD5SUM of the file. If correct, select "Proceed".

After a couple of minutes the MLWG2 restarts with the factory firmware.

## Restore MLWG2 Factory Firmware from bootloader

In case you are not able to restore factory firmware via USB, you may always use the bootloader console.

TODO

### Booting OpenWrt with initramfs image loaded from TFTP

Power up the MLWG2 while keeping the "1" key pressed on the terminal emulator

```
U-Boot 1.1.3 (Dec 30 2013 - 10:32:24)

Board: Ralink APSoC DRAM:  64 MB
relocate_code Pointer at: 83fb4000
enable ephy clock...done. rf reg 29 = 5
SSC disabled.
spi_wait_nsec: 29
spi device id: c2 20 18 c2 20 (2018c220)
find flash: MX25L12805D
raspi_read: from:30000 len:1000
raspi_read: from:30000 len:1000
============================================
Ralink UBoot Version: 4.1.1.0
--------------------------------------------
ASIC 7620_MP (Port5<->None)
DRAM component: 512 Mbits DDR, width 16
DRAM bus: 16 bit
Total memory: 64 MBytes
Flash component: SPI Flash
Date:Dec 30 2013  Time:10:32:24
============================================
icache: sets:512, ways:4, linesz:32 ,total:65536
dcache: sets:256, ways:4, linesz:32 ,total:32768

 ##### The CPU freq = 580 MHZ ####
 estimate memory size =64 Mbytes

Please choose the operation:
   1: Load system code to SDRAM via TFTP.
   2: Load system code then write to Flash via TFTP.
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial.
   9: Load Boot Loader code then write to Flash via TFTP.

You choosed 1
```

Provide the proper parameters to the bootloader - in our example:

* Input device IP: 192.168.64.90
* Input server IP: 192.168.64.106
* Input Linux Kernel filename: openwrt-ramips-mt7620-mlwg2-initramfs-uImage.bin

Watch the serial console for the following messages

```
...
Press the [f] key and hit [enter] to enter failsafe mode
Press the [1], [2], [3] or [4] key and hit [enter] to select the debug level
procd: - early -
procd: - watchdog -
procd: - ubus -
procd: - init -
Please press Enter to activate this console.
```

Press "Enter" to activate the console on the serial port.

```
procd: Console is alive


BusyBox v1.22.1 (2015-01-06 02:30:44 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

procd: - watchdog -
  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 CHAOS CALMER (Bleeding Edge, r43860)
 -----------------------------------------------------
  * 1 1/2 oz Gin            Shake with a glassful
  * 1/4 oz Triple Sec       of broken ice and pour
  * 3/4 oz Lime Juice       unstrained into a goblet.
  * 1 1/2 oz Orange Juice
  * 1 tsp. Grenadine Syrup
 -----------------------------------------------------
root@OpenWrt:/#
```

You can now control the MLWG2 through the shell.

<!-- EOF -->
