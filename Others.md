# Linux
[<TIL](Programming.md)
- [Disable capslock](#Disable capslock)
- [Creating a bootable USB using dd](#Creating a bootable USB using dd)
- [Add suffix to filename](#Add suffix to filename)
- [Check the status of all services](#Check the status of all services)

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

## Watch/capture webcam feed
`$ ffplay /dev/video0`
`$ mpv /dev/video0`
or
`$ mpv av://v4l2:/dev/video0`
`$ vlc v4l2:///dev/video0`

[Source](https://unix.stackexchange.com/questions/3304/how-do-i-watch-my-webcams-feed-in-linux)

## How to install pass extensions
This guide refers to extensions for passwordstore.org:
`git clone https://github.com/browserpass/browserpass-native.git`
`cd browserpass-native`
`docker build -t browserpass-native .`
`docker run --rm -v "$(pwd)":/src browserpass-native browserpass-linux64`
`make BIN=browserpass-linux64 configure`
`sudo make BIN=browserpass-linux64 install`
`cp /usr/lib/browserpass/hosts/chromium/com.github.browserpass.native.json ~/.config/slimjet/NativeMessagingHosts/`

[source](https://github.com/browserpass/browserpass-native#build-using-docker)

## Edit a long command that you previously run
After you run the wrong command (or the one you wan to change)
run `fc`. It will open a vim buffer and paste the last command you run.
Edit it and when you close vim, it will run the command.
Or, use zsh with vim navigation :)

[source](https://dl.suckless.org/slcon/2019/slcon-2019-03-marc_chantreux-acme_changed_my_life.webm)

## Hide tabs in firefox
1. Add a userChrome.css file
 - Open your currently active profile folder
 - Create a new folder named chrome
 - Create a new text file inside the chrome folder named _userChrome.css_
 - Set Firefox to look for userChrome.css at startup
[source](https://www.userchrome.org/how-create-userchrome-css.html)
2. Add css rules to hide the bar:
```css
#main-window[tabsintitlebar="true"]:not([extradragspace="true"]) #TabsToolbar > .toolbar-items {
  opacity: 0;
  pointer-events: none;
}
#main-window:not([tabsintitlebar="true"]) #TabsToolbar {
    visibility: collapse !important;
}
#main-window[tabsintitlebar="true"]:not([extradragspace="true"]) #TabsToolbar .titlebar-spacer {
        border-inline-end: none;
}
```
[source](https://github.com/piroor/treestyletab/wiki/Code-snippets-for-custom-style-rules#for-userchromecss)
