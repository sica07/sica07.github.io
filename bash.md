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
























---
layout: default
---
