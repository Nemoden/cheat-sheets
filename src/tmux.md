tmux
====

[The Tao of tmux](https://leanpub.com/the-tao-of-tmux/read) is free, btw. Happy reading.

`ctrl + b` is default to enter before you enter the commands. `M` = Left Alt key
or ESC key

## Windows

| command | description                                                 |
|---------|-------------------------------------------------------------|
| c       | Create a new window.                                        |
| &       | Kill the current window.                                    |
| n       | Change to the next window.                                  |
| p       | Change to the previous window.                              |
| ,       | Rename the current window.                                  |
| l       | Move to the previously selected window.                     |
| w       | Choose the current window interactively.                    |
| M-n     | Move to the next window with a bell or activity marker.     |
| M-p     | Move to the previous window with a bell or activity marker. |

## Panes

| command | description                                      |
|---------|--------------------------------------------------|
| "       | Split the current pane into two, top and bottom. |
| %       | Split the current pane into two, left and right. |
| x       | Kill the current pane.                           |
| ;       | Move to the previously active pane.              |
| o       | Select the next pane in the current window.}     |
| !       | Break the current pane out of the window.        |
| q       | Briefly display pane indexes.                    |

## Other

| command | description                                                 |
|---------|-------------------------------------------------------------|
| d       | Detach the current client.                                  |
| $       | Rename the current session.                                 |
| [       | Enter copy mode to copy text or view the history.           |
| f       | Prompt to search for text in open windows.                  |
| r       | Force redraw of the attached client.                        |
| L       | Switch the attached client back to the last session.        |
| )       | Next session                                                |
| (       | Previous session                                            |
| $       | Rename Current Session                                      |
| :       | Enter the tmux command prompt                               |
| ?       | List all key bindings                                       |
| f       | Search window titles and goto that window                   |
| i       | Briefly display window information                          |
| r       | Force redraw of the attached client.                        |
| s       | Select a new session for the attached client interactively. |
| t       | Show the time.                                              |
| =       | Choose which buffer to paste interactively from a list.     |
| ]       | Paste the most recently copied buffer of text.              |

## Create a new session

Any of these will work.

```bash
tmux
tmux new
tmux new-session
tmux new -s session_name
```

## Reattach to a session

Any of these commands will work.

```bash
tmux at [-t session-name]
tmux attach [-t session-name]
tmux attach-session [-t session-name]
tmux -L # attach to previous session
```

## List sessions

Any of these commands will work.

```bash
tmux ls
tmux list-sessions
```

## Re-arranging panes

[Following ansers on stackoverflow.com](https://unix.stackexchange.com/questions/124310/how-to-add-a-horizontal-split-to-tmux-window-that-spans-the-whole-width-of-the-p)

General workflow:

`Ctrl+b`, `Alt+[N]` where `N` is 1-5 meaning even-horizontal, even-vertical, main-horizontal, main-vertical, or tiled.

## How to copy and paste

1. Enter copy-mode `C-b [`
2. Move to text, press `Space` to select text, move cursor to highlight text
3. Press `Enter`
4. Back at the command prompt, press `C-b ]` and the text you selected is pasted

To be able to copy to system clipboard in iTerm2 go to the preferences (`Cmd + ,`), -> General -> Selection -> check the box "Applications in terminal may access clipboard".

Reference: [Everything you need to know about Tmux copy paste](http://www.rushiagr.com/blog/2016/06/16/everything-you-need-to-know-about-tmux-copy-pasting/)

## References

* http://www.openbsd.org/cgi-bin/man.cgi?query=tmux&sektion=1#KEY+BINDINGS

## TMUX.conf

There are many things available for tweaking, there are many tmux config files available on github, I personally use my own which I've compiled from different sources: https://github.com/Nemoden/dotfiles/blob/master/.tmux.conf
