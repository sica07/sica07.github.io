# VIM
[<TIL](Programming.md)
- [Favorite colorschemes](#VIM#Favorite colorschemes)
    - [dark](#VIM#Favorite colorschemes#dark)
    - [light](#VIM#Favorite colorschemes#light)
- [Coc](#VIM#Coc)
    - [Python](#VIM#Coc#Python)
- [Xdebug with Vim and Docker](#VIM#Xdebug with Vim and Docker)
- [Close all other](#VIM#Close all other)
- [Spellcheck in vim](#VIM#Spellcheck in vim)

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

## Spellcheck in vim
While on a misspelled word use `z=` to list all variations.
If you want to bring up only the dictionary omnicomplete do a `<C-x><C-k>`.
