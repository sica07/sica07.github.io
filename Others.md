# Linux
[<TIL](Programming.md)

## Disable capslock
Short version (tested in ubuntu):

`setxkbmap -option caps:none`

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

## Copying gpg keys between machines
On the source machine:
`gpg --export ID > public.key`
`gpg --export-secret-key ID > public.key`
On the destination machine:
`gpg --import public.key`
`gpg --import private.key`

[source](https://unix.stackexchange.com/questions/184947)

## Set 10 workspaces in popos
```
gsettings set org.gnome.mutter dynamic-workspaces false
gsettings set org.gnome.desktop.wm.preferences num-workspaces 10
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-1 "['<Super>1']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-1 "['<Super><Shift>1']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-2 "['<Super>2']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-2 "['<Super><Shift>2']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-3 "['<Super>3']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-3 "['<Super><Shift>3']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-4 "['<Super>4']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-4 "['<Super><Shift>4']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-5 "['<Super>5']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-5 "['<Super><Shift>5']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-6 "['<Super>6']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-6 "['<Super><Shift>6']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-7 "['<Super>7']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-7 "['<Super><Shift>7']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-8 "['<Super>8']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-8 "['<Super><Shift>8']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-9 "['<Super>9']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-9 "['<Super><Shift>9']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-10 "['<Super>0']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-10 "['<Super><Shift>0']"
```
[source](https://github.com/pop-os/shell/issues/142#issuecomment-780282706)

## Enable right-click functionality for touchpad
`$ sudo gsettings set org.gnome.desktop.peripherals.touchpad click-method areas`

[src](https://notesread.com/how-to-enable-ubuntu-touchpad-right-click-button-if-it-doesnt-work/)
