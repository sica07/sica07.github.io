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

## Support md files on firefox
Firefox on Linux may not know how to handle markdown files by default.
These mime types are stored in a file indicated by helpers.private_mime_types_file, by default it is ~/.mime.types. Create this file if it does not exist, otherwise edit it, and add the following line:

```
type=text/plain exts=md,mkd,mkdn,mdwn,mdown,markdown, desc="Markdown document"
```
[src](https://github.com/KeithLRobertson/markdown-viewer#support-for-local-files-on-linux)

## i3 with gaps and transparent terminal
### [ i3-gaps ](https://github.com/Airblader/i3)
* install dependencies:
    `$sudo apt-get install gcc make dh-autoreconf libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev xcb libxcb1-dev libxcb-icccm4-dev libyajl-dev libev-dev libxcb-xkb-dev libxcb-cursor-dev libxkbcommon-dev libxcb-xinerama0-dev libxkbcommon-x11-dev libstartup-notification0-dev libxcb-randr0-dev libxcb-xrm0 libxcb-xrm-dev libxcb-shape0-dev`
* install i3-gaps:
```
cd /path/where/you/want/the/repository
```

* clone the repository
```
git clone https://www.github.com/Airblader/i3 i3-gaps
cd i3-gaps
```

* compile & install
```
autoreconf --force --install
rm -rf build/
mkdir -p build && cd build/
```

* Disabling sanitizers is important for release versions!
The prefix and sysconfdir are, obviously, dependent on the distribution.
```
../configure --prefix=/usr --sysconfdir=/etc --disable-sanitizers
make
sudo make install
```

### [ compton ](https://github.com/chjj/compton)
`$ sudo apt-get install compton && compton -bcf`
* **b:** run in background
* **c:** shadow
* **f:** fade
[config file sample](https://ubuntuforums.org/showthread.php?t=2144468&page=3&s=54994dfe49e892664f5b1ae1caf51ffc)

### [xfce-terminal]()
* set transparency to 80%
* set font to _Fira Code_ 14
* set colorscheme to Tango


## How to run both the integrated and the dedicated graphic cards
```
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```
[src](https://askubuntu.com/questions/593938/how-to-run-both-intel-and-nvidia-graphics-card-driver-on-dual-monitor-setup)

## Set default application with xdg
1. find out the MIME type string:
`$ file -i name_of_the.file`

2. find out the name of the application .desktop file:
`$ grep "application_name" -l -r /usr/share/applications`

3. make the assignement:
`$ xdg-mime default application_name file/type`

Example:
```
$ file -i Broken_Blossoms.webm
Broken_Blossoms.webm: video/webm; charset=binary
$ grep "mpv" -l -r /usr/share/applications
/usr/share/applications/mpv.desktop
$ xdg-mime default mpv.desktop video/webm
```

[src](https://askubuntu.com/a/555110)


## Add ssh key to remote server
`ssh-copy-id -i ~/.ssh/id_rsa.pub YOUR_USER_NAME@IP_ADDRESS_OF_THE_SERVER`

or

`mkdir -p /home/user_name/.ssh && touch /home/user_name/.ssh/authorized_keys`

and paste the public key to _authorized_keys_ file.


Don't forget to:
`chmod 700 /home/user_name/.ssh && chmod 600 /home/user_name/.ssh/authorized_keys`
`chown -R username:username /home/username/.ssh`

## kitty missing or unsuitable terminal error
`kitty +kitten ssh myserver`

[src](https://sw.kovidgoyal.net/kitty/faq.html#i-get-errors-about-the-terminal-being-unknown-or-opening-the-terminal-failing-when-sshing-into-a-different-computer)
## Load a small linux distro inside the /boot
"Tiny Core's small enough that I'll often throw the entirety of it in /boot or /boot/EFI
on my Linux desktops as a recovery environment.
```
root (hd0,1)
    kernel /tinycore/vmlinuz tce=sda1/tinycore/tce vga=794
    initrd /tinycore/core.gz
```
in GRUB's shell should do the trick no matter what might lurk in grub.cfg."

[src](https://news.ycombinator.com/item?id=31979269)

