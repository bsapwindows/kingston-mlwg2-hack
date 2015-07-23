Download firmware v2.0.0.6 from http://www.kingston.com/support,or just use the `runme-mlwg2.sh` script in the https://github.com/gmacario/kingston-mlwg2-hack

Copy file `mlwG2_v2.0.0.6.bin` to the root directory of a USB pendrive.

```
gmacario@ITM-GMACARIO-W7 /cygdrive/h
$ ls -la
total 6352
drwxr-xr-x 1 gmacario Domain Users       0 Jan  1  1980 .
dr-xr-xr-x 1 gmacario Domain Users       0 Jan  9 13:19 ..
-rw-r--r-- 1 gmacario Domain Users 6504173 Sep 19 07:03 mlwG2_v2.0.0.6.bin

gmacario@ITM-GMACARIO-W7 /cygdrive/h
$
```

Keep the RESET pin pressed, then turn on the MLGW2.

The firmware update process should take about 5 min.

(optional) Watch the serial console log (need to have [[xxx]])

COMx:57600,8,n,1

```
...
We're now doing FW upgrade...
rm -rf /media/USB1/mlwG2_v2.0.0.6.bin;reboot;
CRC check is OK!
fw_model is WMTM-171GN
fw_version is 0.0
FW model is correct!
Start to run nvram_upgrade function
nvram_upgrade start to sleep...
nvram_upgrade sleep done...
ifconfig: ioctl 0x8913 failed: No such device
ifconfig: ioctl 0x8913 failed: No such device
ifconfig: ioctl 0x8913 failed: No such device
killall: udhcpc: no process killed
killall: wifidetect: no process killed
killall: auto_connect: no process killed
Upgrade to Kernel B

-------Start Image Upgrade--------
INFO:Blinking DIAG LED...
INFO:Erasing kernel partition...
INFO:Burning Kernel&root...
INFO:nLeft = 6504173
INFO:Wrote 64 KB
...
INFO:Wrote 64 KB
INFO:Wrote 15 KB
Upgrade /media/USB1/mlwG2_v2.0.0.6.bin(6351 KB) successfully
semget err:: No such file or directory
_shm2flash:: No such file or directory
Set_bootstate_to_env : 
Reading 4096 bytes......success
Uboot CRC is 3546DF3F, Uboot env CRC is 3546DF3F
Reading 65536 bytes......success
Set_bootstate_to_env : bootstate is [3]
Erase Length: 0x00010000 Bytes
Erasing ......
Erasing ......success
Writing 65536 bytes......
Writing 65536 bytes......success
CAN'T FIND telnettop_server_fwupdate
Unlocking mtd1 ...
Writing from /dev/mtd1_tmp to mtd1 ...  [ ][e][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w][w]

The system is going down NOW!

Sending SIGTERM to all processes

Sending SIGKILL to all processes

Requesting system reboot
Restarting system.



U-Boot 1.1.3 (Dec 30 2013 - 10:32:24)
...
```

TODO: Link complete log 20150109-1315-mlwg2-upgrade-fw-2006.txt

After the firmware is updated the device will reboot

See [[xxx]]

<!-- EOF -->
