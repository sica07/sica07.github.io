# VIM
[<TIL](Programming.md)

## Favorite colorschemes

### dark
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
* nord
* palenight

### light
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


## Coc
### Python
After installing the `python-coc` make sure you run:
`CocCommand <CR>`  and select `python.setInterpreter`


## Xdebug with Vim and Docker

**mandatory settings:**

in the _vdebug.vim_

* **port: 10000**
* **server: ''**
* **path_maps** : {'/path/on/docker': '/path/on/local'}

in the _xdebug.ini_

* **xdebug.remote_host: your.local.machine.ip**
* **xdebug.remote_port: 10000**
* **xdebug.remote_enable: 1**

1. Settings for vdebug.vim:

    ```
    let g:vdebug_options = {
        \ "continuous_mode" : 0,
        \ "port" : 10000,
        \ "server" : '',
        \ "timeout" : 20,
        \ "on_close" : 'detach',
        \ "break_on_open" : 0,
        \ "ide_key" : '',
        \ "debug_window_level" : 0,
        \ "debug_file_level" : 0,
        \ "debug_file" : "~/vdebug.log",
        \ "watch_window_style" : 'expanded',
        \ "marker_default" : '⬦',
        \ "marker_closed_tree" : '▸',
        \ "marker_open_tree" : '▾',
        \ "path_maps" : {'/var/www/html': '/home/marius/Templates/FSPlaza/FSPlaza'}
        \ }
     ```

2. Settings in xdebug.ini:

    ```
    xdebug.remote_enable = 1
    xdebug.remote_autostart = 1
    ;remote_host IP is your local machine IP, the one running vim
    xdebug.remote_host = 192.168.1.116
    ;xdebug.remote_connect_back = 1
    xdebug.idekey = PHPDOCKER
    xdebug.remote_port=10000
    xdebug.remote_log = /var/log/xdebug_remote.log
    ```

**What does not work:**
+ enabling `xdebug.remote_connect_back`
+ setting the `server` for _vdebug_
+ port _9000_ because _fpm_ is using it
+ exposing port _10000_ on the _docker-compose.yml_. No need for that


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
