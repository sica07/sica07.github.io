# Bash
[<TIL](Programming.md)

## Diff in one line from two files
Have you ever done something like this?
```
    $ grep somestring file1 > /tmp/a
    $ grep somestring file2 > /tmp/b
    $ diff /tmp/a /tmp/b
```

That works, but instead you can write:

`diff <(grep somestring file1) <(grep somestring file2)`


## Printing from command line
**Tips:**
* it will print only _PDF_ files
* `-d` is the _destination_ printer and it supports autocomplete with _<tab>_
* `-n` is the number of commands
* `-o` means the options. For all options available try `lpoptions -l`

An example of command:

`$ lp -d HP_Deskjet -n 50 -o OutputMode=FastDraft Documents/sablon_congres.pdf`


## Add suffix to filename
e.g to change the name of a file from old.name to old.name_backup:
`$ mv old.name{,_backup}`


## Saying Yes
`$ yes | rm -r ~/some/dir`

[source](https://twitter.com/dailylaravel/status/1046716110463291392)


## Print all env variables
`$ printenv | less`

[source](https://twitter.com/dailylaravel/status/1046716110463291392)

## Check if a command succeeded in bashscript
You could evaluate the _exit status_:
```
some_command
if [ $? -neq 0 ]; then
    echo "FAIL"
fi
```
Or, the most simple way
```
if some_command; then
    echo "OK"
else
   echo "FAIL"
fi
```
[source](https://askubuntu.com/a/29379)

## ZSH suffix aliases
With suffix aliases, you can launch files with a specific extension (or suffix) in your favorite tool.
To register a suffix alias, we use the `alias -s extension=name-of-the-tool` pattern.
```
alias -s pdf=zathura
alias -s {ape,avi,flv,m4a,mkv,mov,mp3,mp4,mpeg,mpg,ogg,ogm,wav,webm}=mpv
alias -s {jpg,jpeg,png}=feh
alias -s git='git clone'
```
[source](https://thorsten-hans.com/5-types-of-zsh-aliases#suffix-aliases)

## ZSH global aliases
A global alias is aggressive. Once registered, it replaces all occurrences of the alias name with the
specified command. The definition follows the pattern

`alias -g NF='./*(oc[1])'`
This points to the newest file/dir in my current dir, and it is very easy for me to untar a downloaded
file and then cd into it without caring about the name of the file/dir.
`tar xf NF; cd NF`
[source](https://thorsten-hans.com/5-types-of-zsh-aliases#global-aliases)
[source](https://news.ycombinator.com/item?id=23315934)

## Mount folder/filesystem through SSH

Install SSHFS.

`sshfs name@server:/path/to/folder /path/to/mount/point`

[source](https://www.commandlinefu.com/commands/view/193/mount-folderfilesystem-through-ssh)

## Easy access to often executed commands

When using reverse-i-search you have to type some part of the command that you want to retrieve.
It's possible to label your commands and access them easily by pressing ^R and typing the label (should be short and descriptive).

`$ some_very_long_and_complex_command # label`
[source](https://www.commandlinefu.com/commands/view/3384/easy-and-fast-access-to-often-executed-commands-that-are-very-long-and-complex.)

## Show apps that use internet connection at the moment

`lsof -P -i -n`
[source](https://www.commandlinefu.com/commands/view/3542/show-apps-that-use-internet-connection-at-the-moment.-multi-language)

## Save command output to image

`$ some_command | convert label:@- some_name.png`
[source](https://www.commandlinefu.com/commands/view/9104/save-command-output-to-image)
