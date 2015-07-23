From https://forum.openwrt.org/viewtopic.php?pid=254848#p254848 and https://forum.openwrt.org/viewtopic.php?pid=258251#p258251

> Been playing around with stock firmware before trying openwrt.
> Came up with a way to get a shell without opening the device up.
> It's a shell command injection exploiting the firmware upgrade script (/sbin/upgrade.sh) on the root
> of a USB drive create a file called "mlwG2_v;telnetd; .x.x.bin" without the quotes.
>
> From a linux CLI:
>
>     $ touch 'mlwG2_v;telnetd; .x.x.bin'
>
> Insert in the MLWG2 and then boot/reboot.
>
> Give it a while and you should now be able to get a shell using a standard telnet client
> and the username 'admin'.
>
> The default IP is 192.168.201.254.
