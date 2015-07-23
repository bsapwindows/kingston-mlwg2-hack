Introduction
------------

Testing file (from 20150107-1419-OpenWrt_CC_Images):

```
gmacario@kruk:/var/lib/tftpboot$ md5sum openwrt-ramips-mt7620-mlwg2-initramfs-uImage.bin
77cb0c12ac27977698979b860315cc42  openwrt-ramips-mt7620-mlwg2-initramfs-uImage.bin
gmacario@kruk:/var/lib/tftpboot$
```

loaded via TFTP on my Kingston MobileLite Wireless G2.

Prerequisites
-------------

* Serial console for the MLWG2 - See [xxx] for details
* A USB to 3.3V TTL dongle
* A terminal emulator to control the serial console - See [[TODO]]
* A machine configured as a TFTP server - See [[Installing TFTP Server on Ubuntu 14.04.1]]
* Ethernet cable to connect the MLWG2 to the TFTP server

Preparation
-----------

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

Connect the Ethernet port in the MLWG2 to the TFTP server via the Ethernet cable

Booting OpenWrt with initramfs image loaded from TFTP
-----------------------------------------------------

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

Inspecting runtime
------------------

### cat /proc/version

```
root@OpenWrt:/# cat /proc/version
Linux version 3.14.27 (brown@brown) (gcc version 4.8.3 (OpenWrt/Linaro GCC 4.8-2014.04 r43857) ) #12 Wed Jan 7 07:13:00 CST 2015
root@OpenWrt:/#
```

### cat /proc/cmdline

```
root@OpenWrt:/# cat /proc/cmdline
console=ttyS0,57600 rootfstype=squashfs,jffs2
root@OpenWrt:/#
```

### cat /proc/cpuinfo

```
root@OpenWrt:/# cat /proc/cpuinfo
system type             : Ralink MT7620N ver:2 eco:6
machine                 : Kingston MLWG2
processor               : 0
cpu model               : MIPS 24KEc V5.0
BogoMIPS                : 766.77
wait instruction        : yes
microsecond timers      : yes
tlb_entries             : 32
extra interrupt vector  : yes
hardware watchpoint     : yes, count: 4, address/irw mask: [0x0ffc, 0x0ffc, 0x0ffb, 0x0ffb]
isa                     : mips1 mips2 mips32r1 mips32r2
ASEs implemented        : mips16 dsp
shadow register sets    : 1
kscratch registers      : 0
core                    : 0
VCED exceptions         : not available
VCEI exceptions         : not available

root@OpenWrt:/#
```

### cat /proc/filesystems

```
root@OpenWrt:/# cat /proc/filesystems
nodev   sysfs
nodev   rootfs
nodev   ramfs
nodev   bdev
nodev   proc
nodev   tmpfs
nodev   debugfs
nodev   sockfs
nodev   pipefs
nodev   devpts
        squashfs
nodev   jffs2
nodev   overlayfs
nodev   mtd_inodefs
        ext3
        ext2
        ext4
        vfat
        msdos
root@OpenWrt:/#
```

### cat /proc/mounts

```
root@OpenWrt:/# cat /proc/mounts
rootfs / rootfs rw 0 0
proc /proc proc rw,noatime 0 0
sysfs /sys sysfs rw,noatime 0 0
tmpfs /tmp tmpfs rw,nosuid,nodev,noatime 0 0
tmpfs /dev tmpfs rw,relatime,size=512k,mode=755 0 0
devpts /dev/pts devpts rw,relatime,mode=600 0 0
debugfs /sys/kernel/debug debugfs rw,noatime 0 0
root@OpenWrt:/#
```

### cat /proc/mtd

```
root@OpenWrt:/# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00030000 00010000 "u-boot"
mtd1: 00010000 00010000 "u-boot-env"
mtd2: 00010000 00010000 "factory"
mtd3: 00f60000 00010000 "firmware"
mtd4: 00050000 00010000 "user-config"
root@OpenWrt:/#
```

### ls -la /

```
root@OpenWrt:/# ls -la /
drwxr-xr-x   16 root     root             0 Jan  1  1970 .
drwxr-xr-x   16 root     root             0 Jan  1  1970 ..
drwxr-xr-x    2 root     root             0 Jan  7 13:12 bin
drwxr-xr-x    5 root     root           800 Jan  7 13:09 dev
drwxr-xr-x   14 root     root             0 Jan  7 13:09 etc
-rwxr-xr-x    1 root     root            78 Jul 31 13:34 init
drwxr-xr-x   12 root     root             0 Jan  7 13:12 lib
drwxr-xr-x    2 root     root             0 Jan  7 13:12 mnt
drwxr-xr-x    2 root     root             0 Jan  7 13:12 overlay
dr-xr-xr-x   40 root     root             0 Jan  1  1970 proc
drwxr-xr-x    2 root     root             0 Jan  7 13:12 rom
drwxr-xr-x    2 root     root             0 Jan  7 13:12 root
drwxr-xr-x    2 root     root             0 Jan  7 13:12 sbin
dr-xr-xr-x   11 root     root             0 Jan  1  1970 sys
drwxrwxrwt   12 root     root           340 Jan  7 13:09 tmp
drwxr-xr-x    6 root     root             0 Jan  7 13:12 usr
lrwxrwxrwx    1 root     root             4 Jan  7 13:12 var -> /tmp
drwxr-xr-x    4 root     root             0 Jan  7 13:12 www
root@OpenWrt:/#
```

### ls -la /etc

```
root@OpenWrt:/# ls -la /etc
drwxr-xr-x   14 root     root             0 Jan  7 13:09 .
drwxr-xr-x   16 root     root             0 Jan  1  1970 ..
lrwxrwxrwx    1 root     root             7 Jan  7 13:12 TZ -> /tmp/TZ
-rw-r--r--    1 root     root           662 Jan  7 13:08 banner
-rw-r--r--    1 root     root           408 Oct 20 18:55 banner.failsafe
drwxr-xr-x    2 root     root             0 Jan  7 13:12 board.d
-rw-r--r--    1 root     root           338 Jan  7 13:09 board.json
drwxr-xr-x    2 root     root             0 Jan  7 13:09 config
drwxr-xr-x    2 root     root             0 Jan  7 13:12 crontabs
-rw-r--r--    1 root     root            76 Jan  7 13:08 device_info
-rwxr-xr-x    1 root     root          3927 Jan  5 22:37 diag.sh
-rw-r--r--    1 root     root          1368 Jan  7 13:08 dnsmasq.conf
drwx------    2 root     root             0 Jan  7 13:09 dropbear
-rw-r--r--    1 root     root            38 Jan  7 13:08 e2fsck.conf
-rw-r--r--    1 root     root             0 Jan  7 13:09 ethers
-rw-r--r--    1 root     root           352 Jan  7 13:08 firewall.user
lrwxrwxrwx    1 root     root            10 Jan  7 13:12 fstab -> /tmp/fstab
-rw-r--r--    1 root     root           123 Jul 31 13:34 group
-rw-r--r--    1 root     root            20 Jul 31 13:34 hosts
-rw-r--r--    1 root     root           326 Jan  7 13:08 hotplug-preinit.json
drwxr-xr-x    7 root     root             0 Jan  7 13:12 hotplug.d
-rw-r--r--    1 root     root          1657 Jan  7 13:08 hotplug.json
drwxr-xr-x    2 root     root             0 Jan  7 13:12 init.d
-rw-r--r--    1 root     root           101 Jul 31 13:34 inittab
-rw-------    1 root     root         10926 Jan  7 13:07 login.defs
drwxr-xr-x    2 root     root             0 Jan  7 13:12 modules-boot.d
drwxr-xr-x    2 root     root             0 Jan  7 13:12 modules.d
lrwxrwxrwx    1 root     root            12 Jan  7 13:12 mtab -> /proc/mounts
-rw-r--r--    1 root     root           225 Jan  7 13:08 openwrt_release
-rw-r--r--    1 root     root             7 Jan  7 13:08 openwrt_version
-rw-r--r--    1 root     root           873 Jan  7 13:08 opkg.conf
-rw-r--r--    1 root     root           190 Jul 31 13:34 passwd
drwxr-xr-x    2 root     root             0 Jan  7 13:12 ppp
-rwxr-xr-x    1 root     root           952 Jul 31 13:34 preinit
-rw-r--r--    1 root     root           530 Oct 20 18:55 profile
-rw-r--r--    1 root     root          2478 Jul 31 13:34 protocols
drwxr-xr-x    2 root     root             0 Jan  7 13:12 rc.button
-rwxr-xr-x    1 root     root          2354 Jul 31 13:34 rc.common
drwxr-xr-x    2 root     root             0 Jan  7 13:09 rc.d
-rw-r--r--    1 root     root           132 Jul 31 13:34 rc.local
lrwxrwxrwx    1 root     root            16 Jan  7 13:12 resolv.conf -> /tmp/resolv.conf
-rw-r--r--    1 root     root          3017 Jul 31 13:34 services
-rw-------    1 root     root           115 Jul 31 13:34 shadow
-rw-r--r--    1 root     root             9 Jul 31 13:34 shells
-rw-r--r--    1 root     root           913 Aug 20 20:37 sysctl.conf
-rw-r--r--    1 root     root           128 Jul 31 13:34 sysupgrade.conf
drwxr-xr-x    2 root     root             0 Jan  7 13:09 uci-defaults
-rw-------    1 root     root           818 Jan  7 13:08 vsftpd.conf
root@OpenWrt:/#
```

### cat /etc/inittab

```
root@OpenWrt:/# cat /etc/inittab
::sysinit:/etc/init.d/rcS S boot
::shutdown:/etc/init.d/rcS K shutdown
::askconsole:/bin/ash --login
root@OpenWrt:/#
```

### ps

```
root@OpenWrt:/# ps
  PID USER       VSZ STAT COMMAND
    1 root      1432 S    /sbin/procd
    2 root         0 SW   [kthreadd]
    3 root         0 SW   [ksoftirqd/0]
    4 root         0 SW   [kworker/0:0]
    5 root         0 SW<  [kworker/0:0H]
    6 root         0 SW   [kworker/u2:0]
    7 root         0 SW<  [khelper]
    8 root         0 SW   [kworker/u2:1]
   69 root         0 SW<  [writeback]
   72 root         0 SW<  [bioset]
   74 root         0 SW<  [kblockd]
  110 root         0 SW   [kswapd0]
  157 root         0 SW   [fsnotify_mark]
  184 root         0 SW   [spi32766]
  222 root         0 SW<  [deferwq]
  231 root         0 SW   [khubd]
  299 root         0 SW   [kworker/0:2]
  384 root       892 S    /sbin/ubusd
  385 root      1532 S    /bin/ash --login
  554 root         0 SW<  [ipv6_addrconf]
  639 root         0 SW<  [cfg80211]
  866 root      1052 S    /sbin/logd -S 16
  898 root      1552 S    /sbin/netifd
  920 root      1188 S    /usr/sbin/odhcpd
  972 root      1520 S    /usr/sbin/telnetd -F -l /bin/login.sh
  986 root      1536 S    /usr/sbin/uhttpd -f -h /www -r OpenWrt -x /cgi-bin -
  991 root      1012 S    /usr/sbin/vsftpd
 1055 root      1524 S    /usr/sbin/ntpd -n -S /usr/sbin/ntpd-hotplug -p 0.ope
 1092 nobody     972 S    /usr/sbin/dnsmasq -C /var/etc/dnsmasq.conf -k
 1183 root      1148 S    /usr/sbin/dropbear -F -P /var/run/dropbear.1.pid -p
 1244 root      1524 R    ps
root@OpenWrt:/#
```

Verify networking
-----------------

### ifconfig -a

```
root@OpenWrt:/# ifconfig -a
br-lan    Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet addr:192.168.1.1  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fd3c:cc8a:ebd0::1/60 Scope:Global
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1095 errors:0 dropped:0 overruns:0 frame:0
          TX packets:75 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:115531 (112.8 KiB)  TX bytes:8466 (8.2 KiB)

eth0      Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1300 errors:0 dropped:0 overruns:0 frame:0
          TX packets:59 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:165186 (161.3 KiB)  TX bytes:8347 (8.1 KiB)
          Interrupt:5

eth0.1    Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1299 errors:0 dropped:11 overruns:0 frame:0
          TX packets:51 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:141567 (138.2 KiB)  TX bytes:6242 (6.0 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:2606 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2606 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:179966 (175.7 KiB)  TX bytes:179966 (175.7 KiB)

wlan0     Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

root@OpenWrt:/#
```

### cat /etc/config/network

```
root@OpenWrt:/# cat /etc/config/network

config interface 'loopback'
        option ifname 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fd3c:cc8a:ebd0::/48'

config interface 'lan'
        option ifname 'eth0.1'
        option force_link '1'
        option macaddr '00:26:b7:08:e0:a2'
        option type 'bridge'
        option proto 'static'
        option ipaddr '192.168.1.1'
        option netmask '255.255.255.0'
        option ip6assign '60'

root@OpenWrt:/#
```

### cat /etc/config/wireless

```
root@OpenWrt:/# cat /etc/config/wireless
config wifi-device  radio0
        option type     mac80211
        option channel  11
        option hwmode   11g
        option path     '10180000.wmac'
        option htmode   HT20
        # REMOVE THIS LINE TO ENABLE WIFI:
        option disabled 1

config wifi-iface
        option device   radio0
        option network  lan
        option mode     ap
        option ssid     OpenWrt
        option encryption none

root@OpenWrt:/#
```

### Reconfigure networking

From the serial console use the `uci` command to take a static
IP address in your local network and use Google DNS
(replace `x` and `y` appropriately)

```
# uci set network.lan.netmask=255.255.255.0
# uci set network.lan.ipaddr=192.168.x.y
# uci set network.lan.gateway=192.168.x.z
# uci set network.lan.broadcast=192.168.x.255
# uci set network.lan.dns="8.8.4.4 8.8.8.8"
# uci commit network
```

As an alternative, directly edit file `/etc/config/network`

```
# vi /etc/config/network
```

Then restart networking

```
# /etc/init.d/network restart
```

Verify that the network configuration has changed accordingly
(in this example we had `network.lan.ipaddr=192.168.12.50`)

```
root@OpenWrt:/# ifconfig
br-lan    Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet addr:192.168.12.50  Bcast:192.168.12.255  Mask:255.255.255.0
          inet6 addr: fdb9:de23:f3ae::1/60 Scope:Global
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:23 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:2072 (2.0 KiB)  TX bytes:1520 (1.4 KiB)

eth0      Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          inet6 addr: fe80::226:b7ff:fe08:e0a2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:934 errors:0 dropped:1 overruns:0 frame:0
          TX packets:64 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:130692 (127.6 KiB)  TX bytes:9079 (8.8 KiB)
          Interrupt:5

eth0.1    Link encap:Ethernet  HWaddr 00:26:B7:08:E0:A2
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:24 errors:0 dropped:0 overruns:0 frame:0
          TX packets:13 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:2118 (2.0 KiB)  TX bytes:1566 (1.5 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:2126 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2126 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:147326 (143.8 KiB)  TX bytes:147326 (143.8 KiB)

root@OpenWrt:/#
```

Perform some tests:
1. From the MLWG2, try pinging another host in the same subnet -
for instance, the TFTP server
```
# ping <tftp_server_ip>
```
2. Now try pinging a well known server - for instance, Google DNS
```
# ping 8.8.8.8
```
3. From the TFTP server do a port scan of the MLWG2
```
$ sudo nmap <device_ip>
```

Sample result (in this example we had `tftp_server_ip=192.168.12.20` and `device_ip=192.168.12.50`):

```
gmacario@kruk:~$ sudo nmap 192.168.12.50

Starting Nmap 6.40 ( http://nmap.org ) at 2015-01-09 15:21 CET
Nmap scan report for denerg12050.polito.it (192.168.12.50)
Host is up (0.00043s latency).
Not shown: 995 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
23/tcp open  telnet
53/tcp open  domain
80/tcp open  http
MAC Address: 00:26:B7:08:E0:A2 (Kingston Technology Company)

Nmap done: 1 IP address (1 host up) scanned in 90.79 seconds
gmacario@kruk:~$
```

Accessing OpenWrt web interface (LuCI)
--------------------------------------

Prerequisites:

1. Ethernet networking properly setup.
2. A PC on the same Ethernet segment with a recent web browser installed

Browse the device from the PC (example: http://129.168.12.50/)

> Authorization Required
>
> Please enter your usename and password.
>
> * Username: root
> * Password:

Note: the first time you power up the device no password is set and you can just select "Login".

You will be reminded

> No password set!
>
> There is no password set on this router.
> Please configure a root password to protect the web interface and enable SSH.
>
> Go to password configuration...

From LuCI: System > Administration

> Router Password
>
> Changes the administrator password for accessing the device
>
> * Password:
> * Confirmation:

Provide a new password, then select "Save & Apply".

Flashing OpenWrt image on the MLWG2
-----------------------------------

From LuCI: System > Backup / Flash Firmware

Flash new firmware image - Upload a sysupgrade-compatible image here to replace the running firmware. Check "Keep settings" to retain the current configuration (requires an OpenWrt compatible firmware image).

* Keep settings: Yes
* Image: .../20150107-1419-OpenWrt_CC_images/openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin

Then select "Flash image..."

> Flash Firmware - Verify
>
> The flash image was uploaded.
> Below is the checksum and file size listed, compare them with the original
> file to ensure data integrity.
> Click "Proceed" below to start the flash procedure.
>
> * Checksum: 5e413331373e88978fde018724b723a0
> * Size: 6.75 MB (15.38 MB available)
> * Configuration files will be kept.

Verify that the displayed checksum matches the one returned by "md5sum _filename_".

Select "Proceed"

> DO NOT POWER OFF THE DEVICE

(optional) Verify running processes from the serial console

```
root@OpenWrt:/# ps
  PID USER       VSZ STAT COMMAND
    1 root      1496 S    /sbin/procd
    2 root         0 SW   [kthreadd]
    3 root         0 SW   [ksoftirqd/0]
    4 root         0 SW   [kworker/0:0]
    5 root         0 SW<  [kworker/0:0H]
    6 root         0 SW   [kworker/u2:0]
    7 root         0 SW<  [khelper]
    8 root         0 SW   [kworker/u2:1]
   69 root         0 SW<  [writeback]
   72 root         0 SW<  [bioset]
   74 root         0 SW<  [kblockd]
  110 root         0 SW   [kswapd0]
  157 root         0 SW   [fsnotify_mark]
  184 root         0 RW   [spi32766]
  222 root         0 SW<  [deferwq]
  231 root         0 SW   [khubd]
  384 root      1532 S    /bin/ash --login
  553 root         0 SW<  [ipv6_addrconf]
  638 root         0 SW<  [cfg80211]
  985 root      1520 S    /usr/sbin/telnetd -F -l /bin/login.sh
 3853 root         0 SW   [kworker/0:2]
 5377 root      1624 S    {sysupgrade} /bin/sh /sbin/sysupgrade /tmp/firmware.
 5414 root      1624 S    {sysupgrade} /bin/sh /sbin/sysupgrade /tmp/firmware.
 5415 root       960 R    mtd -j /tmp/sysupgrade.tgz write - firmware
 5421 root      1516 S    cat /tmp/firmware.img
 5423 root      1524 R    ps
root@OpenWrt:/#
```

FIXME: The system has rebooted and reverted to the factory RaLink firmware (WHY???)

```
root@OpenWrt:/#
root@OpenWrt:/#
root@OpenWrt:/#
root@OpenWrt:/# [  582.140000] reboot: Restarting system


U-Boot 1.1.3 (Dec 30 2013 - 10:32:24)

Board: Ralink APSoC DRAM:  64 MB
relocate_code Pointer at: 83fb4000
enable ephy clock...done. rf reg 29 = 5
SSC disabled.
******************************
Software System Reset Occurred
******************************
spi_wait_nsec: 29
```


Possible workarounds:

1. Install OpenWrt from the stock MLWG2 bootloader
2. Use commandline (sysupgrade) tool

### Install OpenWrt from the stock MLWG2 bootloader

Logged into the TFTP server, copy the image file on the TFTP root directory

```
$ cd /var/lib/tftpboot
$ sudo scp  .../20150107-1419-OpenWrt_CC_images/openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin .
```

(Optional) Verify the image checksum

```
ubuntu@ubuntu:/var/lib/tftpboot$ md5sum openwrt-ramips-mt7620-mlwg2-*
5e413331373e88978fde018724b723a0  openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin
ubuntu@ubuntu:/var/lib/tftpboot$
```

Make sure the MLWG2 is fully charged and connected to the power supply.

Power up the MLWG2 while keeping the "2" key pressed on the terminal emulator

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

You choosed 2
                                                                                        0
raspi_read: from:40028 len:6


2: System Load Linux Kernel then write to Flash via TFTP.
 Warning!! Erase Linux in Flash then burn new one. Are you sure?(Y/N)
```

Think twice, then type `Y`

```
 Please Input new ones /or Ctrl-C to discard
        Input device IP (192.168.64.90) ==:192.168.64.90
        Input server IP (192.168.64.106) ==:192.168.64.106
        Input Linux Kernel filename (openwrt-ramips-mt7620-mlwg2-initramfs-uImage.bin) ==:openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin
```

The file is downloaded then written to the Flash memory.
When finished, then the device will reboot and load the new kernel.

Verify that the new kernel is loaded - note that the first time the initialization may take longer

```
...
[   27.030000] device eth0 entered promiscuous mode
[   27.030000] br-lan: port 1(eth0.1) entered forwarding state
[   27.030000] br-lan: port 1(eth0.1) entered forwarding state
[   29.030000] br-lan: port 1(eth0.1) entered forwarding state
[   29.410000] jffs2_scan_eraseblock(): End of filesystem marker found at 0x0
[   29.450000] jffs2_build_filesystem(): unlocking the mtd device... done.
[   29.450000] jffs2_build_filesystem(): erasing all blocks after the end marker... done.
[  100.800000] jffs2: notice: (1100) jffs2_build_xattr_subsystem: complete building xattr subsystem, 0 of xdatum (0 unchecked, 0 orphan) and 0 of xref (0 dead, 0 orphan) found.
procd: - init complete -
```

You should eventually be able to get a shell by pressing the "Enter" key
on the serial console.

```
BusyBox v1.22.1 (2015-01-06 02:30:44 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

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

Congratulations! You just upgraded your Flash memory with OpenWrt.

### Use commandline (sysupgrade) tool

```
root@OpenWrt:~# sysupgrade
Usage: /sbin/sysupgrade [<upgrade-option>...] <image file or URL>
       /sbin/sysupgrade [-q] [-i] <backup-command> <file>

upgrade-option:
        -d <delay>   add a delay before rebooting
        -f <config>  restore configuration from .tar.gz (file or url)
        -i           interactive mode
        -c           attempt to preserve all changed files in /etc/
        -n           do not save configuration over reflash
        -T | --test
                     Verify image and config .tar.gz but do not actually flash.
        -F | --force
                     Flash image even if image checks fail, this is dangerous!
        -q           less verbose
        -v           more verbose
        -h | --help  display this help

backup-command:
        -b | --create-backup <file>
                     create .tar.gz of files specified in sysupgrade.conf
                     then exit. Does not flash an image. If file is '-',
                     i.e. stdout, verbosity is set to 0 (i.e. quiet).
        -r | --restore-backup <file>
                     restore a .tar.gz created with sysupgrade -b
                     then exit. Does not flash an image. If file is '-',
                     the archive is read from stdin.
        -l | --list-backup
                     list the files that would be backed up when calling
                     sysupgrade -b. Does not create a backup file.

root@OpenWrt:~#
```

TODO: sysupgrade --test xxx.bin

Booting OpenWrt from Flash
--------------------------

Power up MLWG2 without pressing any other button.

Watch the serial console for the following message

```
[   27.640000] br-lan: port 1(eth0.1) entered forwarding state
[   29.640000] br-lan: port 1(eth0.1) entered forwarding state
procd: - init complete -
```

The configuration should be the same as from last boot.

Assuming that you had changed the IP address and set the root password,
try to login via ssh

```
$ ssh root@192.168.64.50
```

You should be able to login to a shell after providing the root password.

Halt the system by typing

```
root@OpenWrt:~# sync
root@OpenWrt:~# sync
root@OpenWrt:~# halt
root@OpenWrt:~#
```

Watch the serial console for the following messages

```
...
procd: - shutdown -
[  178.520000] br-lan: port 1(eth0.1) entered disabled state
[  178.520000] device eth0.1 left promiscuous mode
[  178.540000] device eth0 left promiscuous mode
[  178.540000] br-lan: port 1(eth0.1) entered disabled state
[  178.590000] IPv6: ADDRCONF(NETDEV_UP): eth0.1: link is not ready
procd:[  182.030000] reboot: System halted
```

The blue LEDs should then turn off.

Boot in failsafe mode
---------------------

Reference: http://wiki.openwrt.org/doc/howto/generic.failsafe

Power up MLWG2 while keeping the "Reset" pin pressed.

Watch the serial console for the following message

```
[   27.640000] br-lan: port 1(eth0.1) entered forwarding state
[   29.640000] br-lan: port 1(eth0.1) entered forwarding state
procd: - init complete -
```

TODO: Not sure whether the Reset pin works.

Triggering failsafe mode by pressing "f" key and "enter" on the serial console
does seem to work, though.

```
procd: - preinit -
[    9.410000] 8021q: adding VLAN 0 to HW filter on device eth0
[    9.520000] random: mktemp urandom read with 79 bits of entropy available
Press the [f] key and hit [enter] to enter failsafe mode
Press the [1], [2], [3] or [4] key and hit [enter] to select the debug level
f
- failsafe -


BusyBox v1.22.1 (2015-01-06 02:30:44 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

ash: can't access tty; job control turned off
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
================= FAILSAFE MODE active ================
special commands:
* firstboot          reset settings to factory defaults
* mount_root     mount root-partition with config files

after mount_root:
* passwd                         change root's password
* /etc/config               directory with config files

for more help see:
http://wiki.openwrt.org/doc/howto/generic.failsafe
=======================================================

root@(none):/# [   42.710000] random: nonblocking pool is initialized

root@(none):/#
```

Testing Wi-Fi
-------------

### Configure and enable Wi-Fi

Referemce: http://wiki.openwrt.org/doc/uci/wireless

```
$ uci set wireless.@wifi-iface[0].ssid=OpenWRT-MLWG2
$ uci set wireless.@wifi-iface[0].encryption=psk2
$ uci set wireless.@wifi-iface[0].key="MySecret"
$ uci set wireless.@wifi-device[0].disabled=0
$ uci commit wireless
$ wifi
```

Verify configuration

```
root@OpenWrt:~# uci show wireless
wireless.radio0=wifi-device
wireless.radio0.type=mac80211
wireless.radio0.channel=11
wireless.radio0.hwmode=11g
wireless.radio0.path=10180000.wmac
wireless.radio0.htmode=HT20
wireless.radio0.disabled=0
wireless.@wifi-iface[0]=wifi-iface
wireless.@wifi-iface[0].device=radio0
wireless.@wifi-iface[0].network=lan
wireless.@wifi-iface[0].mode=ap
wireless.@wifi-iface[0].encryption=none
wireless.@wifi-iface[0].ssid=OpenWRT-MLWG2
root@OpenWrt:~#
```

Now connect to Wi-Fi SSID "OpenWRT-MLWG2" ==> Success!

You can also verify status on LuCI: http://192.168.64.90/
Network > Wifi

Tested with
* uci set wireless.@wifi-iface[0].encryption=none (default)
* uci set wireless.@wifi-iface[0].encryption=psk2

Installing additional packages
------------------------------

### From commandline

```
# opkg update
# opkg install git
```

Test the correct installation of the `git` command

```
root@OpenWrt:/# git ls-remote git://git.openwrt.org/openwrt.git
778ec528fe706365846a5787a0e47a8da3f9363c        HEAD
778ec528fe706365846a5787a0e47a8da3f9363c        refs/heads/master
778ec528fe706365846a5787a0e47a8da3f9363c        refs/remotes/trunk
root@OpenWrt:/#
```

### From LuCI

Browse LuCI homepage (example: http://192.168.64.90/), then

System > Software

Download and install package: git

Saving OpenWrt configuration
----------------------------

LuCI: System > Backup / Flash Firmware

Then select "Download backup" to generate an archive and
download it from the browser.

Result saved as ~/Downloads/backup-OpenWrt-2015-01-11.tar.gz

```
$ tar tvfz ~/Downloads/backup-OpenWrt-2015-01-11.tar.gz
-rw-r--r-- root/root       806 2015-01-07 14:07 etc/config/ddns
-rw-r--r-- root/root     10856 2015-01-07 14:07 etc/config/ddns.sample
-rw-r--r-- root/root       738 2015-01-07 14:09 etc/config/dhcp
-rw-r--r-- root/root       134 2015-01-07 14:07 etc/config/dropbear
-rw-r--r-- root/root      3887 2015-01-07 14:08 etc/config/firewall
-rw-r--r-- root/root       151 2015-01-07 14:09 etc/config/fstab
-rw-r--r-- root/root       624 2015-01-07 14:09 etc/config/luci
-rw-r--r-- root/root       595 2015-01-07 14:09 etc/config/minidlna
-rw-r--r-- root/root       529 2015-01-07 14:13 etc/config/network
-rw-r--r-- root/root       594 2015-01-10 14:56 etc/config/system
-rw-r--r-- root/root      2265 2015-01-07 14:08 etc/config/transmission
-rw-r--r-- root/root       900 2015-01-07 14:09 etc/config/ucitrack
-rw-r--r-- root/root       635 2015-01-07 14:09 etc/config/uhttpd
-rw-r--r-- root/root       329 2015-01-10 18:37 etc/config/wireless
-rw------- root/root       457 2015-01-07 14:10 etc/dropbear/dropbear_dss_host_key
-rw------- root/root       805 2015-01-07 14:10 etc/dropbear/dropbear_rsa_host_key
-rw-r--r-- root/root       123 2014-07-31 15:34 etc/group
-rw-r--r-- root/root        20 2014-07-31 15:34 etc/hosts
-rw-r--r-- root/root       101 2014-07-31 15:34 etc/inittab
-rw-r--r-- root/root       190 2014-07-31 15:34 etc/passwd
-rw-r--r-- root/root       530 2014-10-20 20:55 etc/profile
-rw-r--r-- root/root       132 2014-07-31 15:34 etc/rc.local
-rw------- root/root       132 2015-01-10 14:48 etc/shadow
-rw-r--r-- root/root         9 2014-07-31 15:34 etc/shells
-rw-r--r-- root/root       913 2014-08-20 22:37 etc/sysctl.conf

$
```

Testing "Reset" pin
-------------------

First save your custom OpenWrt configuration (see above).

With the device running, insert a paper clip to press and hold the reset button
for about 10 seconds then release.
If you are watching the console....it should say "FACTORY RESET"

This should reset the device to the OpenWrt defaults....

==> (2015-01-11 09:16 CET) Does not work on my MLWG2
(tested with image 2015-01-09 14:19 image by ldpinney `openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin`
built from OpenWrt Chaos Calmer r43860)

Alternatively, from LuCI
LuCI: System > Backup / Flash Firmware
select "Perform reset" to reset to defaults.
Confirm when requested, the browser will show

> Waiting for changes to be applied...

Then reboot with default configuration.

Testing GPIO (again)
--------------------

From http://wiki.openwrt.org/doc/devel/add.new.device

### Testing GPIO LEDs

TODO

### Testing GPIO buttons


Execute the `/sbin/buttons.sh` command from serial console
after booting in failsafe mode:

```
BusyBox v1.22.1 (2015-01-06 02:30:44 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

ash: can't access tty; job control turned off
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
================= FAILSAFE MODE active ================
special commands:
* firstboot          reset settings to factory defaults
* mount_root     mount root-partition with config files

after mount_root:
* passwd                         change root's password
* /etc/config               directory with config files

for more help see:
http://wiki.openwrt.org/doc/howto/generic.failsafe
=======================================================

root@(none):/# [   41.940000] random: nonblocking pool is initialized

root@(none):/#
root@(none):/# /sbin/buttons.sh
[GPIO0] value 0
sh: write error: Device or resource busy
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio1/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio1/value': N[   62.040000] rt2880-pinmux pinctrl.1: pin 3 is not set to gpio mux
o such file or d[   62.060000] rt2880-pinmux pinctrl.1: request() failed for pin 3
irectory
[GPIO1[   62.080000] rt2880-pinmux pinctrl.1: pin-3 (pio:3) status -22
] value
sh: write error: Invalid argument
sh:[   62.100000] rt2880-pinmux pinctrl.1: pin 4 is not set to gpio mux
 write error: De[   62.100000] rt2880-pinmux pinctrl.1: request() failed for pin 4
vice or resource[   62.130000] rt2880-pinmux pinctrl.1: pin-4 (pio:4) status -22
 busy
/sbin/buttons.sh: line 10: can't create /[   62.140000] rt2880-pinmux pinctrl.1: pin 5 is not set to gpio mux
sys/class/gpio/g[   62.160000] rt2880-pinmux pinctrl.1: request() failed for pin 5
pio2/direction: [   62.170000] rt2880-pinmux pinctrl.1: pin-5 (pio:5) status -22
nonexistent directory
cat: can't open '/sys/cla[   62.190000] rt2880-pinmux pinctrl.1: pin 6 is not set to gpio mux
ss/gpio/gpio2/va[   62.210000] rt2880-pinmux pinctrl.1: request() failed for pin 6
lue': No such fi[   62.220000] rt2880-pinmux pinctrl.1: pin-6 (pio:6) status -22
le or directory
[GPIO2] value
sh: write error[   62.240000] rt2880-pinmux pinctrl.1: pin 7 is not set to gpio mux
: Invalid argume[   62.260000] rt2880-pinmux pinctrl.1: request() failed for pin 7
nt
sh: write er[   62.270000] rt2880-pinmux pinctrl.1: pin-7 (pio:7) status -22
ror: Invalid argument
/sbin/buttons.sh: line 10[   62.290000] rt2880-pinmux pinctrl.1: pin 8 is not set to gpio mux
: can't create /[   62.310000] rt2880-pinmux pinctrl.1: request() failed for pin 8
sys/class/gpio/g[   62.320000] rt2880-pinmux pinctrl.1: pin-8 (pio:8) status -22
pio3/direction: nonexistent directory
cat: can'[   62.340000] rt2880-pinmux pinctrl.1: pin 9 is not set to gpio mux
t open '/sys/cla[   62.360000] rt2880-pinmux pinctrl.1: request() failed for pin 9
ss/gpio/gpio3/va[   62.370000] rt2880-pinmux pinctrl.1: pin-9 (pio:9) status -22
lue': No such file or directory
[   62.390000] rt2880-pinmux pinctrl.1: pin 10 is not set to gpio mux

sh: write error[   62.410000] rt2880-pinmux pinctrl.1: request() failed for pin 10
: Invalid argume[   62.410000] rt2880-pinmux pinctrl.1: pin-10 (pio:10) status -22
nt
sh: write error: Invalid argument
/sbin/but[   62.440000] rt2880-pinmux pinctrl.1: pin 11 is not set to gpio mux
tons.sh: line 10[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 11
: can't create /[   62.440000] rt2880-pinmux pinctrl.1: pin-11 (pio:11) status -22
sys/class/gpio/gpio4/direction: nonexistent dire[   62.440000] rt2880-pinmux pinctrl.1: pin 12 is not set to gpio mux
ctory
cat: can'[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 12
t open '/sys/cla[   62.440000] rt2880-pinmux pinctrl.1: pin-12 (pio:12) status -22
[   62.440000] rt2880-pinmux pinctrl.1: pin 13 is not set to gpio mux

[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 13

sh: write error[   62.440000] rt2880-pinmux pinctrl.1: pin-13 (pio:13) status -22
: Invalid argument
sh: write error: Invalid arg[   62.440000] rt2880-pinmux pinctrl.1: pin 14 is not set to gpio mux
ument
/sbin/but[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 14
tons.sh: line 10[   62.440000] rt2880-pinmux pinctrl.1: pin-14 (pio:14) status -22
: can't create /sys/class/gpio/gpio5/direction: [   62.440000] rt2880-pinmux pinctrl.1: pin 15 is not set to gpio mux
nonexistent dire[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 15
ctory
cat: can'[   62.440000] rt2880-pinmux pinctrl.1: pin-15 (pio:15) status -22
t open '/sys/class/gpio/gpio5/value': No such fi[   62.440000] rt2880-pinmux pinctrl.1: pin 16 is not set to gpio mux
[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 16

[   62.440000] rt2880-pinmux pinctrl.1: pin-16 (pio:16) status -22

sh: write error: Invalid argument
sh: write er[   62.440000] rt2880-pinmux pinctrl.1: pin 17 is not set to gpio mux
ror: Invalid arg[   62.440000] rt2880-pinmux pinctrl.1: request() failed for pin 17
ument
/sbin/but[   62.770000] rt2880-pinmux pinctrl.1: pin-17 (pio:17) status -22
tons.sh: line 10: can't create /sys/class/gpio/g[   62.790000] rt2880-pinmux pinctrl.1: pin 18 is not set to gpio mux
pio6/direction: [   62.810000] rt2880-pinmux pinctrl.1: request() failed for pin 18
nonexistent dire[   62.810000] rt2880-pinmux pinctrl.1: pin-18 (pio:18) status -22
ctory
cat: can't open '/sys/class/gpio/gpio6/va[   62.840000] rt2880-pinmux pinctrl.1: pin 19 is not set to gpio mux
lue': No such fi[   62.860000] rt2880-pinmux pinctrl.1: request() failed for pin 19
[   62.860000] rt2880-pinmux pinctrl.1: pin-19 (pio:19) status -22

[GPIO6] value
sh: write error: Invalid argume[   62.890000] rt2880-pinmux pinctrl.1: pin 20 is not set to gpio mux
nt
sh: write er[   62.910000] rt2880-pinmux pinctrl.1: request() failed for pin 20
ror: Invalid arg[   62.910000] rt2880-pinmux pinctrl.1: pin-20 (pio:20) status -22
ument
/sbin/buttons.sh: line 10: can't create /[   62.940000] rt2880-pinmux pinctrl.1: pin 21 is not set to gpio mux
sys/class/gpio/g[   62.960000] rt2880-pinmux pinctrl.1: request() failed for pin 21
pio7/direction: [   62.960000] rt2880-pinmux pinctrl.1: pin-21 (pio:21) status -22
nonexistent directory
cat: can't open '/sys/cla[   62.990000] rt2880-pinmux pinctrl.1: pin 22 is not set to gpio mux
ss/gpio/gpio7/va[   63.010000] rt2880-pinmux pinctrl.1: request() failed for pin 22
lue': No such fi[   63.010000] rt2880-pinmux pinctrl.1: pin-22 (pio:22) status -22
le or directory
[GPIO7] value
sh: write error[   63.040000] rt2880-pinmux pinctrl.1: pin 23 is not set to gpio mux
: Invalid argume[   63.060000] rt2880-pinmux pinctrl.1: request() failed for pin 23
nt
sh: write er[   63.060000] rt2880-pinmux pinctrl.1: pin-23 (pio:23) status -22
ror: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio8/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio8/value': No such file or directory
[GPIO8] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio9/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio9/value': No such file or directory
[GPIO9] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio10/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio10/value': No such file or directory
[GPIO10] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio11/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio11/value': No such file or directory
[GPIO11] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio12/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio12/value': No such file or directory
[GPIO12] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio13/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio13/value': No such file or directory
[GPIO13] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio14/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio14/value': No such file or directory
[GPIO14] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio15/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio15/value': No such file or directory
[GPIO15] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio16/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio16/value': No such file or directory
[GPIO16] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio17/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio17/value': No such file or directory
[GPIO17] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio18/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio18/value': No such file or directory
[GPIO18] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio19/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio19/value': No such file or directory
[GPIO19] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio20/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio20/value': No such file or directory
[GPIO20] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio21/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio21/value': No such file or directory
[GPIO21] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio22/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio22/value': No such file or directory
[GPIO22] value
sh: write error: Invalid argument
sh: write error: Invalid argument
/sbin/buttons.sh: line 10: can't create /sys/class/gpio/gpio23/direction: nonexistent directory
cat: can't open '/sys/class/gpio/gpio23/value': No such file or directory
[GPIO23] value
sh: write error: Invalid argument
root@(none):/#
```

There is a lot of noise but apparently the important message is on stdout,
let us redirect stdout to a file and ignore the rest.

```
# /sbin/buttons.sh >/tmp/buttons_off.txt
```

Repeat while keeping the "Reset" pin pressed with a paper clip

```
# /sbin/buttons.sh >/tmp/buttons_reset_pressed.txt
```

Then compare the results (we have no diff so let us use ls and md5sum)

```
root@(none):/# diff -u /tmp/buttons_off.txt /tmp/buttons_reset_pressed.txt
ash: diff: not found
root@(none):/# ls -la /tmp/buttons_off.txt /tmp/buttons_reset_pressed.txt
-rw-r--r--    1 root     root           375 Jan  1 00:10 /tmp/buttons_off.txt
-rw-r--r--    1 root     root           375 Jan  1 00:10 /tmp/buttons_reset_pressed.txt
root@(none):/# md5sum /tmp/buttons_off.txt /tmp/buttons_reset_pressed.txt
fc4a160a97402ee443b730ab82745986  /tmp/buttons_off.txt
fc4a160a97402ee443b730ab82745986  /tmp/buttons_reset_pressed.txt
root@(none):/#
```

Contents of each file

```
root@(none):/# cat /tmp/buttons_off.txt
[GPIO0] value 0
[GPIO1] value
[GPIO2] value
[GPIO3] value
[GPIO4] value
[GPIO5] value
[GPIO6] value
[GPIO7] value
[GPIO8] value
[GPIO9] value
[GPIO10] value
[GPIO11] value
[GPIO12] value
[GPIO13] value
[GPIO14] value
[GPIO15] value
[GPIO16] value
[GPIO17] value
[GPIO18] value
[GPIO19] value
[GPIO20] value
[GPIO21] value
[GPIO22] value
[GPIO23] value
root@(none):/#
```

Inspecting `/sys/class/gpio`

```
root@(none):/# ls -la /sys/class/gpio/
drwxr-xr-x    2 root     root             0 Jan  1 00:03 .
drwxr-xr-x   19 root     root             0 Jan  1 00:00 ..
--w-------    1 root     root          4096 Jan  1 00:03 export
lrwxrwxrwx    1 root     root             0 Jan  1 00:03 gpiochip0 -> ../../devices/10000000.palmbus/10000600.gpio/gpio/gpiochip0
lrwxrwxrwx    1 root     root             0 Jan  1 00:03 gpiochip24 -> ../../devices/10000000.palmbus/10000638.gpio/gpio/gpiochip24
lrwxrwxrwx    1 root     root             0 Jan  1 00:03 gpiochip40 -> ../../devices/10000000.palmbus/10000660.gpio/gpio/gpiochip40
lrwxrwxrwx    1 root     root             0 Jan  1 00:03 gpiochip72 -> ../../devices/10000000.palmbus/10000688.gpio/gpio/gpiochip72
--w-------    1 root     root          4096 Jan  1 00:03 unexport
root@(none):/#
```

## Testing latest MLWG2 firmware (2015-01-11 17:15 CET)

### Flashing latest MLWG2 firmware (2015-01-11 17:15 CET)

```
$ cd build.local/20150111-1715-ldpinney_latest_files/
$ md5sum openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin
c5fb8e44af71640a31e427d3e569aa6c  openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin
$
```

LuCI: System > Backup / Flash Firmware
* Keep settings: Yes
* Image: openwrt-ramips-mt7620-mlwg2-squashfs-sysupgrade.bin

Then select "Flash image..."

Verify chechsum, then select "Proceed"

When writing flash is complete the device will reboot and the new image will be loaded.

```
BusyBox v1.22.1 (2015-01-11 06:16:20 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 CHAOS CALMER (Bleeding Edge, r43927)
 -----------------------------------------------------
  * 1 1/2 oz Gin            Shake with a glassful
  * 1/4 oz Triple Sec       of broken ice and pour
  * 3/4 oz Lime Juice       unstrained into a goblet.
  * 1 1/2 oz Orange Juice
  * 1 tsp. Grenadine Syrup
 -----------------------------------------------------
root@OpenWrt:/#
```

### Testing buttons

TODO

<!-- EOF -->
