# Linux
[<TIL](Programming.md)

## Disable capslock
Try creating the file 10-nocaps.conf with the following content:

```
Section "InputClass"
        Identifier             "keyboard-layout"
        MatchIsKeyboard        "on"
        Option "XkbOptions"    "ctrl:nocaps"
EndSection
```

...and place it in either /etc/X11/xorg.conf.d or /usr/share/X11/xorg.conf.d (depending on which folder your distro uses) and log out and back in again to test.


## Creating a bootable USB using dd
**Warning:** This will irrevocably destroy all data on /dev/sdx. To restore the USB drive as an empty, usable storage device after using
the Arch ISO image, the ISO 9660 filesystem signature needs to be removed by running `wipefs --all /dev/sdx` as root, before repartitioning
and reformating the USB drive.

**Tip:** Find out the name of your USB drive with `lsblk` or `lsusb` and `# fdisk -l` to see the partition of the USB. Make sure that it is not mounted.

Run the following command, replacing `/dev/sdx` with your drive, e.g. `/dev/sdb`. (Do not append a partition number, so do not use something
like `/dev/sdb1`)

`# dd bs=4M if=path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync`

**Tip:** If the UEFI version of the USB's Arch ISO hangs or is unable to load, try repeating the **dd** medium creation process
on the same USB drive one or more times.

[source](https://wiki.archlinux.org/index.php/USB_flash_installation_media#Using_dd)

## Add suffix to filename
e.g to change the name of a file from old.name to old.name_backup:
`mv old.name{,_backup}`


## Check the status of all services
`$ sudo service --status-all`

[source](https://github.com/jbranchaud/til/blob/master/devops/check-the-status-of-all-services.md)