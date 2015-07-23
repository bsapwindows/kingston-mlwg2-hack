Prerequisite: create file "xxx" on root directory of USB pendrive (see [[Login to a command shell on MLWG2]]), then

```
$ telnet 192.168.201.254
```
Login as admin to get a BusyBox shell.

```
mobilelite.home login: admin


BusyBox v1.12.1 (2014-06-19 15:22:45 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

#
```

#### cat /proc/version
Result
```
Linux version 2.6.36+ (root@CVS2) (gcc version 3.4.2) #5 Thu Jun 19 15:46:56 CST 2014
```

#### cat /proc/cmdline
Result
```
console=ttyS1,57600n8 root=/dev/ram0 console=ttyS0
```

#### cat /proc/cpuinfo
Result
```
system type             : Ralink SoC
processor               : 0
cpu model               : MIPS 24Kc V5.0
BogoMIPS                : 386.04
wait instruction        : yes
microsecond timers      : yes
tlb_entries             : 32
extra interrupt vector  : yes
hardware watchpoint     : yes, count: 4, address/irw mask: [0x0004, f8, 0x0e63]
ASEs implemented        : mips16 dsp
shadow register sets    : 1
core                    : 0
VCED exceptions         : not available
VCEI exceptions         : not available
```

#### cat /proc/filesystems
Result
```
nodev   sysfs
nodev   rootfs
nodev   bdev
nodev   proc
nodev   tmpfs
nodev   sockfs
nodev   usbfs
nodev   pipefs
nodev   devpts
        ext2
        ext3
        ext4
nodev   ramfs
        vfat
        iso9660
nodev   jffs2
nodev   fuse
        fuseblk
nodev   fusectl
        udf
nodev   mtd_inodefs
        ufsd
```

#### cat /proc/mounts
Result
```
rootfs / rootfs rw 0 0
proc /proc proc rw,relatime 0 0
none /var ramfs rw,relatime 0 0
none /tmp ramfs rw,relatime 0 0
none /media ramfs rw,relatime 0 0
none /sys sysfs rw,relatime 0 0
none /dev/pts devpts rw,relatime,mode=600 0 0
none /proc/bus/usb usbfs rw,relatime 0 0
none /etc ramfs rw,relatime 0 0
mdev /dev ramfs rw,relatime 0 0
devpts /dev/pts devpts rw,relatime,mode=600 0 0
/dev/mtdblock5 /etc/config jffs2 rw,relatime 0 0
/dev/sda1 /media/USB1 vfat rw,relatime,fmask=0000,dmask=0000,allow_utime=0022,codepage=cp950,iocharset=utf8,shortname=mixed,errors=remount-ro 0 0
```

#### cat /proc/mtd
Result
```
dev:    size   erasesize  name
mtd0: 00030000 00010000 "Bootloader"
mtd1: 00010000 00010000 "Config"
mtd2: 00010000 00010000 "Factory"
mtd3: 007b0000 00010000 "KernelA"
mtd4: 007b0000 00010000 "KernelB"
mtd5: 00050000 00010000 "User_CFG"
```

#### ls -la /
Result
```
drwxr-xr-x    9 0        0               0 etc_ro
drwxr-xr-x    2 0        0               0 lighttpd_www
lrwxrwxrwx    1 0        0              11 init -> bin/busybox
drwxr-xr-x    7 0        0               0 usr
drwxr-xr-x    2 0        0               0 bin
drwxr-xr-x    5 0        0               0 www
drwxr-xr-x    4 0        0               0 lib
drwxr-xr-x    7 0        0               0 var
drwxr-xr-x    4 0        0               0 tmp
drwxr-xr-x    5 0        0               0 etc
drwxr-xr-x   11 0        0               0 sys
drwxr-xr-x    3 0        0               0 media
drwxr-xr-x    3 0        0               0 dev
drwxr-xr-x    2 0        0               0 sbin
dr-xr-xr-x   71 0        0               0 proc
drwxr-xr-x    2 0        0               0 home
drwxr-xr-x    2 0        0               0 mnt
drwxr-xr-x   18 0        0               0 ..
drwxr-xr-x   18 0        0               0 .
```

#### ls -la /etc
Result
```
lrwxrwxrwx    1 0        0               7 TZ -> /tmp/TZ
-rw-r--r--    1 0        0            1119 certSrv.pem
drwxr-xr-x    3 0        0               0 config
-rw-r--r--    1 0        0             371 fstab
lrwxrwxrwx    1 0        0              10 hosts -> /tmp/hosts
drwxr-xr-x    2 0        0               0 miniupnpd
drwxr-xr-x    3 0        0               0 ppp
-rw-r--r--    1 0        0             887 privkeySrv.pem
lrwxrwxrwx    1 0        0              16 resolv.conf -> /tmp/resolv.conf
lrwxrwxrwx    1 0        0              16 secrets.tdb -> /tmp/secrets.tdb
-rw-r--r--    1 0        0              92 services
-rw-r--r--    1 0        0             143 mdev.conf
-rw-rw-rw-    1 0        0             126 passwd
-rw-rw-rw-    1 0        0              52 group
lrwxrwxrwx    1 0        0              23 smb.conf -> /tmp/samba/lib/smb.conf
drwxr-xr-x   18 0        0               0 ..
drwxr-xr-x    5 0        0               0 .
```

#### ls -la /etc_ro
Result
```
drwxr-xr-x    2 0        0               0 linuxigd
drwxr-xr-x    2 0        0               0 web
drwxr-xr-x    5 0        0               0 Wireless
-rw-r--r--    1 0        0             325 motd
-rw-r--r--    1 0        0              79 lld2d.conf
drwxr-xr-x    2 0        0               0 usb
-rw-r--r--    1 0        0            9662 icon.large.ico
drwxr-xr-x    2 0        0               0 wlan
drwxr-xr-x    2 0        0               0 xml
-rw-r--r--    1 0        0              45 inittab
-rw-r-xr-x    1 0        0            6696 rcS
drwxr-xr-x    5 0        0               0 ppp
-rw-r--r--    1 0        0            9662 icon.ico
drwxr-xr-x   18 0        0               0 ..
drwxr-xr-x    9 0        0               0 .
```

#### cat /etc_ro/inittab
Result
```
::sysinit:/etc_ro/rcS
ttyS1::respawn:/bin/sh
```

#### cat /etc_ro/rcS
Result
```
#!/bin/sh
RC_FILE=/tmp/rc.conf
#insmod /lib/modules/jnl.ko
insmod /lib/modules/ufsd.ko
mount -a
# to make etc ramfs
cp -r /etc /var
mount -t ramfs none /etc
mv /var/etc/* /etc

# for USB
mount -t ramfs mdev /dev
echo "/sbin/mdev" > /proc/sys/kernel/hotplug
mkdir /dev/pts
mount -t devpts devpts /dev/pts

echo "sd[a-z][1-9] 0:0 0660 */sbin/automount.sh \$MDEV" > /etc/mdev.conf
echo "sd[a-z] 0:0 0660 */sbin/automount_basic.sh \$MDEV" >> /etc/mdev.conf


mdev -s
mknod   /dev/video0      c       81      0
mknod   /dev/spiS0       c       217     0
mknod   /dev/i2cM0       c       218     0
mknod   /dev/rdm0        c       254     0
mknod   /dev/flash0      c       200     0
mknod   /dev/swnat0      c       210     0
mknod   /dev/hwnat0      c       220     0
mknod   /dev/acl0        c       230     0
mknod   /dev/ac0         c       240     0
mknod   /dev/mtr0        c       250     0
mknod   /dev/gpio        c       252     0
mknod   /dev/pcm0        c       233     0
mknod   /dev/i2s0        c       234     0
mknod   /dev/cls0        c       235     0

mkdir -p /etc/config
# Mount JFFS2 NTD partition
/bin/mount -t jffs2 /dev/mtdblock5 /etc/config
mkdir -p /var/run
#for syslogd
mkdir -p /var/log
cat /etc_ro/motd
/usr/sbin/nvram_recover
##nvram_daemon&
pbc_detect &
#
# system booting
gpio l 11 0 4000 0 1 4000  # off
gpio l 7 0 4000 0 1 4000   # off

#For v4.2.0.1 (10M problem)
mii_mgr -s -p 0 -r 0 -v 0x3100
mii_mgr -s -p 1 -r 0 -v 0x3100
mii_mgr -s -p 2 -r 0 -v 0x3100
mii_mgr -s -p 3 -r 0 -v 0x3100
mii_mgr -s -p 4 -r 0 -v 0x3100

# initialize nvram
/usr/sbin/nvram init
. $RC_FILE

# Check MAC
/usr/sbin/gt_utils check_MAC

# initialize WAN state parameters
/usr/sbin/nvram set DHCPState=4
/usr/sbin/nvram set PPPoE1State=9
#/usr/sbin/nvram set PPPoE2State=9
#/usr/sbin/nvram set PPPoE3State=9
#/usr/sbin/nvram set PPPoE4State=9
#/usr/sbin/nvram set PPPoE5State=9

#Initialize this nvram value for ECO
checkntp_status=`nvram get checkntp_gettime`
now_eco_mode_status=`nvram get now_eco_mode`
AOSS_exception_status=`nvram get AOSS_exception`
InAOSSECO_status=`nvram get InAOSSECO`
return_page_status=`nvram get return_page`
if [ "$checkntp_status" != "0" ] ; then
        nvram set checkntp_gettime="0"
fi

if [ "$now_eco_mode_status" != "0" ] ; then
        nvram set now_eco_mode="0"
fi

if [ "$AOSS_exception_status" != "0" ] ; then
        nvram set AOSS_exception="0"
fi

if [ "$InAOSSECO_status" != "0" ] ; then
        nvram set InAOSSECO="0"
fi

if [ "$return_page_status" != "0" ] ; then
        nvram set return_page="0"
fi

#Initialize Wan Check Status
/usr/sbin/nvram set WanCheckStatus=0

#--Gemtek Add for fix MAC learning issue. For request, disable MAC learning function
router_mode=`nvram get RouterMode`
if [ "$router_mode" != 0 ] ; then
        switch reg w 0x94 1000
fi

# Ralink recommends to set this for SmartBits test: 3f502b28+bit8~9:0 => 3f50 2 [1011] 28 => 3f50 2 [1000] 28
#switch reg w 0xc8 3f502828

# Initialize LAN interface
/usr/sbin/gt_utils lan_init

#Firmware version and release date
/usr/sbin/gt_utils version

#Set Time zone
/usr/shell/settimezone.sh

# Set default System date
/bin/date `nvram get SystemDefaultTime`

#echo "nameserver 168.95.1.1" > /etc/resolv.conf

# loopback interface
/sbin/ifconfig lo up

# Start klogd and syslogd
touch /tmp/log

#--Gemtek Make console log level=4 to prevent Router reboot.
/sbin/klogd -c 4 &
/usr/sbin/gt_utils syslog start

#Enable Watchdog daemon
mknod /tmp/watchdog c 10 130
wd_keepalive -c /etc_ro/watchdog.conf -b

#Reset AOSS status to default
nvram set WIFIAOSSActive="0"

#Always enable WMM
nvram set WIFIWmmCapable=1

# Detect push button
#/usr/sbin/pbc_detect &

# Initialize semaphores
firewall sem_init
/usr/sbin/gt_utils wan_sem_init

# Initialize Firewalli
#router_mode=`nvram get RouterMode`
if [ "$router_mode" != 0 ] ; then
        firewall init
fi

#if [ "$router_mode" == "1" ] ; then
#       nvram set WIFIApCliEnable="0"
#fi

# Initialize WLAN interface and start LAN Services
/usr/sbin/gt_utils wlan_init

#Jayo 2010/01/15: insert ipv6 passthrough module after wlan init,and before lan service starting
insmod /lib/modules/2.6.21/kernel/net/ipv6passthru/ipv6passthru.ko
nvram set ethernet_wan="0"
/usr/sbin/gt_utils lan_service start

# Add Boot Log
/usr/sbin/gt_utils boot_log

# Dtetct WAN Link
#/usr/sbin/link_detect &

#wan_mode=`nvram get WanMethod`
#if [ "$wan_mode" != "4" ] && [ "$router_mode" != "0" ] ; then
#       /usr/sbin/wan_check 2
#elif [ "$router_mode" == 0 ] ; then
#       /usr/sbin/wan_check 2
#fi

##Gemtek for fixed boot miss usb and sd issue
nvram set scsi_count=""
boot_usbsd.sh

if [ `nvram get easy_test` == "1" ]; then
        easytest &
        iperf -s &
fi

#GUI
#cp /usr/sbin/html.tar.bz2 /tmp/
#cd /tmp/
#tar -jxvf html.tar.bz2
#rm -f html.tar.bz2
#mkdir -p /media/config
power_check &
cd /www; /usr/sbin/httpd &
/bin/server &
/bin/ssl_server 443 &
#/bin/ap_serv -i br0 &
# DIAG LED
/usr/sbin/led 9 off

# ECO
#eco_mode=`nvram get ECOEnable`
#if [ "$eco_mode" == 1 ] ; then
        #killall eco
        #/usr/sbin/eco &
#fi

# Wifi detect
wifi_mode=`nvram get Wifi_on`
if [ "$wifi_mode" == 1 ] ; then
        killall wifidetect
        nvram set GetChannel="0"
        nvram set wifi_detect_success="0"
        /usr/sbin/wifidetect &
        auto_connect &
fi

#ICMP redirect
echo 0 > /proc/sys/net/ipv4/conf/all/send_redirects
echo 0 > /proc/sys/net/ipv4/conf/default/send_redirects
echo 0 > /proc/sys/net/ipv4/conf/br0/send_redirects

#booting finish:

gpio l 11 0 4000 0 1 4000   # off
gpio l 7 4000 0 1 0 4000    # on


# Do FW upgrade at boot time if image exists

#if [ -f /media/sda1/SkyShare4973.bin ]; then
    #nvram upgrade /media/sda1/SkyShare4973.bin;
#fi
#telnetd &

#Gemtek add for apache
mkdir -p /tmp/logs
chmod 777 /tmp/logs
/usr/local/apache2/bin/htpasswd -b -c /tmp/logs/user.passwd guest guest
widrive_aloha &
##mkdir -p /media/sda1/
##mount -o umask=000 /dev/sda1  /media/sda1/
##mkdir -p /media/sdb1/
##mount -o umask=000 /dev/sdb1  /media/sdb1/
##mkdir -p /media/sdc1/
##mount -o umask=000 /dev/sdc1  /media/sdc1/
##mkdir -p /media/config/
#/usr/local/apache2/bin/apachectl start &
lighttpd -f /usr/lighttpd.conf &
#Gemtek end

#Gemtek add for samba
ln -sf /tmp/samba/lib/smb.conf /etc/smb.conf
##sambaset &
sambaset `ls /media` &
#Gemtek end

#Gemtek add for dlna
DMS_set &
#gemtek end

#Gemtek add for auto shundown
shutdown_check &
#gemtek end

iwpriv ra0 set GreenAP=1

#usb_autocheck.sh &


#FW_buf=`ls /media/*/Kingston_widrive_v*.*.*.bin`
#nvram upgrade $FW_buf
uboot_env  Get_bootstate_to_nvram
bootstate=`nvram get bootstate`
if [ "$bootstate" == "1" ]; then
        `nvram set bootstate=0`
        uboot_env  Set_bootstate_to_env
elif [ "$bootstate" == "3" ]; then
        `nvram set bootstate=2`
        uboot_env  Set_bootstate_to_env
fi

upgrade.sh

checkeasy.sh

link_detect &

led 43 off
```

#### cat /etc/fstab
Result
```
proc            /proc           proc    defaults 0 0
none            /var            ramfs   defaults 0 0
none            /tmp            ramfs   defaults 0 0
none            /media          ramfs   defaults 0 0
none            /sys            sysfs   default  0 0
none            /dev/pts        devpts  default  0 0
none            /proc/bus/usb   usbfs   defaults 0 0
```

#### cat /etc/passwd
Result
```
admin::0:0:Adminstrator:/:/bin/sh
guest::501:501:guest:/:/media/sda1/home/guest
guest::501:501:guest:/:/media/sda1/home/guest
```

#### cat /etc/group
Result
```
admin:x:0:admin
guest:x:501:guest
guest:x:501:guest
```

#### cat /etc/services
Result
```
ftp     21/tcp
tftp    69/udp
telnet  23/tcp
ssh     22/tcp
httpd   80/tcp
udhcpd  67/udp
ntp     123/udp
```

#### cat /etc/mdev.conf
Result
```
sd[a-z][1-9] 0:0 0660 */sbin/automount.sh $MDEV
sd[a-z] 0:0 0660 */sbin/automount_basic.sh $MDEV
ttyUSB0 0:0 0660 @/sbin/autoconn3G.sh connect
```

#### cat /sbin/upgrade.sh
Result
```
#! /bin/sh

#FW_buf=`ls /media/*/Kingston_widrive_v*.*.*.bin`
FW_buf=`ls /media/*/mlw5200fw_v*.*.*.bin`
nvram upgrade $FW_buf

FW_buf2=`ls /media/*/mlwG2_v*.*.*.bin`
nvram upgrade $FW_buf2

exit 0
```

#### cat /sbin/checkeasy.sh
Result
```
#! /bin/sh

if [ `nvram get easy_test` -eq 0 ]; then
        if [ `ls /media/*/gemtek_easytest.txt | wc -l` -ne 0 ]; then
                easytest &
                iperf -s &
        fi
fi

exit 0
```

<!-- EOF -->
