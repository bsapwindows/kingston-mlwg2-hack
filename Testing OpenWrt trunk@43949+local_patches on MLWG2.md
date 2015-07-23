Introduction
============

This page documents a few tests made on the [OpenWrt](https://openwrt.org/) image
for the [Kingston MWLG2](http://wiki.openwrt.org/toh/kingston/mlwg2).

Prerequisites
-------------

* One Kingston MLWG2 device
* Power supply for the MLWG2 (preferred before flashing the device)
* The firmware image `openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin`
created as described in [[Rebuilding OpenWrt for MLWG2]]
* A controlling PC
* Ethernet cable
* (Optional) A terminal emulator connected to MLWG2 serial console as
described [here](http://wiki.openwrt.org/toh/kingston/mlwg2).

Install the new OpenWrt image on the MWLG2
==========================================

There are two cases here, depending whether the MLWG2 is running
the stock firmware, or OpenWrt was already flashed to the device

Install the OpenWrt image on a stock MWLG2
------------------------------------------

You need to do it via BootLoader.

TODO

Upgrade the MLWG2 with the new OpenWrt image
--------------------------------------------

If a previous OpenWrt image was already flashed on the MLWG2,
you may easily upgrade the MLWG2 from OpenWrt itself.

Upgrade the MLWG2 with the new image (2015-01-12 18:05)

```
gmacario@mv-linux-powerhorse:~⟫ cd ~/easy-build/build-openwrt/shared/output-20150112-1805
gmacario@mv-linux-powerhorse:~/easy-build/build-openwrt/shared/output-20150112-1805⟫ md5sum *.bin
a193f29cdf4af086f3d50e42f34c1e9b  openwrt-ramips-mt7620-mlwg2-initramfs-uImage.bin
c7a3fd5f50e8053160a71af58f864300  openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin
gmacario@mv-linux-powerhorse:~/easy-build/build-openwrt/shared/output-20150112-1805⟫ grep mlwg2 md5sums  | md5sum -c -
openwrt-ramips-mt7620-mlwg2-initramfs-uImage.bin: OK
openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin: OK
gmacario@mv-linux-powerhorse:~/easy-build/build-openwrt/shared/output-20150112-1805⟫
```

Power up MLWG2, wait until serial console shows

```
procd: - init complete -
```

From the PC browse LuCI (default: http://192.168.1.1/)

* Username: root
* Password: xxxx

LuCI: System > Backup / Flash Firmware

Flash new firmware image

* Keep settings: Yes
* Image: E:\temp\output-20150112-1805/openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin

then select "Flash Firmware"

> Flash Firmware - Verify
>
> The flash image was uploaded. Below is the checksum and file size listed,
> compare them with the original file to ensure data integrity.
> Click "Proceed" below to start the flash procedure.
>
> * Checksum: c7a3fd5f50e8053160a71af58f864300
> * Size: 6.75 MB (15.38 MB available)
> * Configuration files will be kept.

OK, checksum matches ==> Select "Proceed"

Watch the serial console to make sure the new kernel is loaded when the MLWG2 reboots.

In case you requested to keep the original configuration,
after a while LuCI will ask to login again:

> * Username: root
> * Password: xxx
>
> Powered by LuCI patches Branch (git-15.011.60122-a850efc) OpenWrt Chaos Calmer r43949

OK, verified that the expected OpenWrt version (Chaos Chalmer r43949) is running.

From a PC let us log into the MWLG2

In our case we have restored our custom configuration
* changed device IP to 192.168.64.90 (default IP is 192.168.1.1)
* have also set root password (so telnet is disabled while SSH is enabled)

```
$ ssh root@192.168.64.90
root@192.168.64.90's password:


BusyBox v1.22.1 (2015-01-12 17:55:47 UTC) built-in shell (ash)
Enter 'help' for a list of built-in commands.

_______                     ________        __
|       |.-----.-----.-----.|  |  |  |.----.|  |_
|   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
|_______||   __|_____|__|__||________||__|  |____|
|__| W I R E L E S S   F R E E D O M
-----------------------------------------------------
CHAOS CALMER (Bleeding Edge, r43949)
-----------------------------------------------------
* 1 1/2 oz Gin            Shake with a glassful
* 1/4 oz Triple Sec       of broken ice and pour
* 3/4 oz Lime Juice       unstrained into a goblet.
* 1 1/2 oz Orange Juice
* 1 tsp. Grenadine Syrup
-----------------------------------------------------
root@OpenWrt:~#
```

Test the MLWG2 with OpenWrt
===========================

Power up the MLWG2, wait until serial console shows

```
procd: - init complete -
```

From another PC, log into the MLWG2.

You may log into the MLWG2 via one of the following:
1. Serial console (preferred): From the serial console, type "Enter"
2. telnet (if no root password was configured in OpenWrt)
3. ssh (if root password configured)

The default IP address for the Ethernet interface is 192.168.1.1.

Notice that you may use the `uci` command to reconfigure networking
to take a free IP address of your local Ethernet.

```
$ ssh root@192.168.64.90
root@192.168.64.90's password:


BusyBox v1.22.1 (2015-01-12 17:55:47 UTC) built-in shell (ash)
Enter 'help' for a list of built-in commands.

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 CHAOS CALMER (Bleeding Edge, r43949)
 -----------------------------------------------------
  * 1 1/2 oz Gin            Shake with a glassful
  * 1/4 oz Triple Sec       of broken ice and pour
  * 3/4 oz Lime Juice       unstrained into a goblet.
  * 1 1/2 oz Orange Juice
  * 1 tsp. Grenadine Syrup
 -----------------------------------------------------
root@OpenWrt:~#
```

Inspect runtime
---------------

### cat /proc/version

```
TODO
```

### cat /proc/cmdline

```
TODO
```

### cat /proc/cpuinfo

```
TODO
```

### cat /proc/filesystems

```
TODO
```

### cat /proc/meminfo

```
TODO
```

### cat /proc/mounts

```
TODO
```

### cat /proc/mtd

```
TODO
```

### ls -la /

```
TODO
```

### ls -la /etc

```
TODO
```

### cat /etc/inittab

```
TODO
```

### cat /etc/init.d/rcS

```
TODO
```

Verify networking
-----------------

### ifconfig -a

```
TODO
```

Verify GPIO
-----------------

### TODO

```
TODO
```

<!-- EOF -->
