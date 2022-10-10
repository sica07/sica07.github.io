# VIM
[<TIL](Programming.md)

## Favorite colorschemes

### dark
* nord ❤️
* palenight
* challenger_deep
* atom
* dusk
* dracula
* thornbird
* itg_flat
* material-theme
* base16-material-dark
* desertEx
* twilight
* gruvbox

### light
* paper ❤️
* _whitebox_
* base16-tomorrow
* base16-github
* base16-one-light
* eclipse
* lightcolors
* pyte
* beauty256
* hemisu
* kate


## Xdebug3 with Vim

### Install _vimspector.vim_
```
$ mkdir ~/.config/vimspector/configurations/linux/_all
$ cd ~/.config/nvim/plugged/vimspector
$ ./install_gadget.py --basedir $HOME/.config/vimspector --force-enable-php --update-gadget-config
$ touch ~/.config/vimspector/configurations/linux/_all/config.json
```
Add the following content to config.json:
```
{
  "configurations" : {

    "attach": {
      "adapter": "vscode-php-debug",
      "default": true,
      "filetypes": [ "php" ],
      "configuration": {
        "name": "Listen for XDebug",
        "type": "php",
        "request": "launch",
        "port": 9003,
        "stopOnEntry": false,
        "pathMappings": {
          "/var/www/html/": "${workspaceRoot}"
        }
      }
    }
  }
}
```
### How to use it
When starting a debugging session:
1. Add a breakpoint where interested <F9>
2. open a file in the root folder and start `:call vimspector#Launch()`
3. From here on everything should work as expected


## Close all other
Close all other windows/splits

`:only`

[source](https://github.com/jbranchaud/til/blob/master/vim/close-all-other-windows.md)

Close all other tabs
`:tabonly`

## Zoom on one window
If in vertical split mode:
`Ctrl+w |`
If in horizontal split mode:
`Ctrl+w _`
Reload the initial split structure:
`Ctrl+w =`

## Indentation tips
* `==` indent current line or `X==` indend current line and next X
* `=%` indent to end of the method
* `=ap` indend arround paragraph

[src](https://t.co/Y7E4kv3BrT)

## Spellcheck in vim
While on a misspelled word use `z=` to list all variations.
If you want to bring up only the dictionary omnicomplete do a `<C-x><C-k>`.

## Running selected code
Beacause the `!` is actually a pipe, we can visually select a block of code
`phpinfo();`
and then, in ex mode, run the command:
`:'<,'>!php -a`
or with sqlite3:
```SQL
CREATE TABLE tbl (
    f1 [varchar](varchar)(30) primary key,
    f2 text,
    f3 real
)
```
and then, in ex mode, run the command:
`:'<,'>!sqlite3 nameof.db`
and will create the table inside sqlite db.

[source](https://dl.suckless.org/slcon/2019/slcon-2019-03-marc_chantreux-acme_changed_my_life.webm)


## Open a file and go to specific line
`$ nvim +10 test.py`
or if you already are inside vim:
`:e +10 test.py`

[source](https://jdhao.github.io/2019/12/21/nifty_nvim_techniques_s6/#open-a-file-and-go-to-specific-line)

## Abbreviations
The command for Insert mode abbreviations looks like this:
```
:iabbrev [<expr>] [<buffer>] {abbreviation} {expansion}

    <expr> - stands for Vimscript expression to create the expansion.
    <buffer> - means that it only applies to the current buffer.
    {abbreviation} - is the thing you type, or your “trigger”
    {expansion} - is your final outcome
```
Anything that’s inside a [] is optional.

Run this command in your Vim: `:iabbrev teh the`. Now enter the Insert mode and type teh end. As soon as you hit Space after typing the teh, Vim will replace it with the.
Of course, you can store all of your abbreviations in your .vimrc file.
Other examples
```
iabbrev @@ hi@jovica.org
iabbrev <expr> ddd strftime('%c')
iabbrev cc /*<CR><CR>/<Up>
```
Let’s say we want to make one abbreviation for printing a string in Python and Java.
```
"python
autocmd Filetype python :iabbrev ppp print("");<left><left><left>
"java
autocmd FileType java :iabbrev ppp System.out.println("");<left><left><left>
```
[soruce](https://jovica.org/posts/vim_abbreviations/)

## Fugitive
## Common git tasks
`:Gwrite` - git add current file
`:Gread` - git checkout current file (revert to last checked in version)
`:Gmove` - git move/rename the current file

### Merge conflicts with Gdiff

`(master)$ git merge feature`

| **target(HEAD/master)** | **working copy** | **merge (feature)** |
|-------------------------|------------------|---------------------|
| builds:                 | builds:          | builds:             |
| main:                   | main:            | main:               |
| env:                    | env:             | env:                |
| -CGO_ENABLED=1          | <<<<<HEAD        | -CGO_ENABLED=0      |
| -SOMEENV=true           | -CGO_ENABLED=1   | -OTHERNEV=off       |
| ----------              | -SOMEENV=true    | -------             |
| --------                | =====            | -----               |
| -----                   | -CGO_ENABLED=0   | ------              |
| ----                    | -OTHERNEV=off    | ----                |
| -----                   | >>>>>master      | -----               |
| binary: fed             | binary: fed      | binary: fed         |

**TIP:** The arrows of the diff syntax point to the specific window of the split
(<<<HEAD left window (//2), >>>master right window(//3))

`:diffget 2` fetch hunk from the target parent (left)
`:diffget 3` fetch hunk from the merge parent (right)
`:Gwrite!` write/stage the current file to the index

Or use `:diffput` and put the hunk from the current window (target/merge)
to the working copy. Using it like this, the `:diffput` command requiers no
argument, so the shorthand `:dp` command can be used.

**Tip** in a 2-way diff, the diffget command require no argument so the
shorthand `:do` command can be used.

`[c` jump to previous hunk
`]c` jump to nex hunk
[src](http://vimcasts.org/episodes/fugitive-vim-resolving-merge-conflicts-with-vimdiff/)

## Automated file templates
```
autocmd BufNewFile readme.md 0r ~/skeletons/readme.md
autocmd BufNewFile *.sh 0r ~/skeletons/bash.sh
```
* `autocmd` – run this automatically on some event
* `BufNewFile` – this is Vim’s new file event
* `readme.md` – this is the pattern you want the new file to match
* `0r` – read into the buffer starting at line 0, the first line
* `~/skeletons/readme.md` – the file to read in
[src](https://vimtricks.com/p/automated-file-templates/)

## The path
To add directories to path just use `:set path=`
Common cases:
`set path=.,**` - add current file and all directories relative to current working directory
`set path=.,app/**` - add current file and all children relative to ./app folder

[src](https://www.youtube.com/watch?v=Gs1VDYnS-Ac&t=1069s)

## Find which plugin modified a variable

`:verbose set VARIABLE`

to show the value of VARIABLE and the file that least changed it, e.g.:
```
	:verbose set formatoptions
	  formatoptions=jtcql
	        Last set from ~/.vimrc

	:verbose set commentstring
	  commentstring=# %s
        	Last set from /usr/share/vim/vim80/ftplugin/gitcommit.vim
```
[src](https://leahneukirchen.org/TIL)

## Copy a line an paste it under the current line
* `:<line number>copy.` or using the alias `:<line number>t.`
* `:10,20t.` copy lines 10 to 20 and paste them below
* `:+8t.` copye the line 8 lines _bellow_ the current line and paste it below
* `:-8t.` copye the line 8 lines _above_ the current line and paste it below
* `:t8` copy the current line and paste it below line 8

[src](https://vimtricks.com/p/vim-copy-line/)

## Find and run action over multiple files
1. `:Ggrep needle` - Ggrep or grep or Ack or whatever search plugin
2. `:cdo s/needle/replacement/gc | update` - itereates over every result from the quickfix list and
interactively does the replacement

[src](https://vimtricks.com/p/interactive-replace-across-files/)

