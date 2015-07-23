Install the following packages:

```
$ sudo apt-get install tftpd-hpa tftp-hpa
```

The files to be served must be placed under `/var/lib/tftpboot` and must be world readable - for example:

```
$ sudo cp ~/Downloads/my_uImage.bin /var/lib/tftpboot/
$ sudo chmod 444 /var/lib/tftpboot/my_uImage.bin
```

You can test it from the command line using the `tftp` client:

```
gmacario@kruk:~$ tftp localhost
tftp> get my_uImage.bin
Received 6715839 bytes in 1.0 seconds
tftp> quit
gmacario@kruk:~$ cmp /var/lib/tftpboot/my_uImage.bin my_uImage.bin
gmacario@kruk:~$
```

If the `cmd` command produces no outputs, you have correctly downloaded the file from the TFTP server.

<!-- EOF -->