# Bash
[<TIL](Programming.md)
- [Diff in one line from two files](#Diff in one line from two files)
- [Printing from command line](#Printing from command line)
- [Add suffix to filename](#Add suffix to filename)
- [Saying Yes](#Saying Yes)
- [Print all env variables](#Print all env variables)

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
```bash
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
```bash
alias -s pdf=zathura
alias -s {ape,avi,flv,m4a,mkv,mov,mp3,mp4,mpeg,mpg,ogg,ogm,wav,webm}=mpv
alias -s {jpg,jpeg,png}=feh
```
[source](https://thorsten-hans.com/5-types-of-zsh-aliases#suffix-aliases)
