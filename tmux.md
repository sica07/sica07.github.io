# Tmux
[<TIL](Programming.md)

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

## Fix issue with italics inside tmux
1. Create tmux-256color.terminfo
   ```
   tmux-256color|screen with 256 colors and italic,
    sitm=\E[3m, ritm=\E[23m,
    use=screen-256color,
   tic tmux-256color.terminfo
```

2. `set -g default-terminal "tmux-256color"` in .tmux.conf
[src](https://github.com/tmux/tmux/issues/377#issuecomment-212541169)

## Create a new named session with root directory
`$ tmux new -s custom_name -c ~/custom/directory`

[src](https://github.com/jbranchaud/til/blob/master/tmux/change-base-directory-of-existing-session.md)

## Hide status bar
`:set status off`

[source](https://github.com/jbranchaud/til/blob/master/tmux/hiding-the-status-bar.md)

## Working with sessions
List all sessions:
`tmux ls`

Attach to a session:
`tmux a -t old_session`

Create new session:
`tmux new -s session_name`

Switch to previous session:
`ctrl-b )`

Switch to next session:
`ctrl-b (`

Detach the session:
`:detach`

Kill a specific session:
`tmux kill-session -t old_session`

Kill all tmux open sessions:
`:kill-server`

Kill all _other_sessions:
`tmux kill-session -a`

[source](https://www.usenix.org/sites/default/files/conference/protected-files/lisa19_maheshwari.pdf)
[source](https://askubuntu.com/a/868194)




