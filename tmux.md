# Tmux
[<TIL](knowledge.md)
- [Fix vim color issues in tmux](#Fix vim color issues in tmux)
- [Create a new named session with root directory](#Create a new named session with root directory)
- [Hide status bar](#Hide status bar)

## Fix vim color issues in tmux
1. In **.tmux.conf** add:
   ```
    set -g default-terminal 'screen-256color'
    set -ga terminal-overrides ',xterm-256color:Tc'
   ```
Where 'xterm-256color' should be whatever your $PATH is outside of tmux.
2. In **.vimrc** add:
   ```
    set termguicolors
    let &t_8f = "\<Esc>[38;2;%lu;%lu;%lum"
    let &t_8b = "\<Esc>[48;2;%lu;%lu;%lum"
   ```
This will tell vim the escape codes. It's explained in `:h xterm-true-color`.


## Create a new named session with root directory
`$ tmux new -s custom_name -c ~/custom/directory`

[inspiration](https://github.com/jbranchaud/til/blob/master/tmux/change-base-directory-of-existing-session.md)

## Hide status bar
`:set status off`

[source](https://github.com/jbranchaud/til/blob/master/tmux/hiding-the-status-bar.md)
