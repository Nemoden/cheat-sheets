## Useful bash / zsh shortcuts

### Move cursor

| Command   | Description                                                  |
|-----------|--------------------------------------------------------------|
| Ctrl + a  | Go to the beginning of the line (Home)                       |
| Ctrl + e  | Go to the End of the line (End)                              |
| Alt + b   | Back (left) one word                                         |
| Alt + f   | Forward (right) one word                                     |
| Ctrl + f  | Forward one character                                        |
| Ctrl + b  | Backward one character                                       |
| Ctrl + xx | Toggle between the start of line and current cursor position |

### Edit

| Command   | Description                                                                       |
|-----------|-----------------------------------------------------------------------------------|
| Ctrl + L  | Clear the Screen, similar to the clear command                                    |
| Ctrl + u  | Cut the line before the cursor position                                           |
| Alt + Del | Delete the Word before the cursor                                                 |
| Alt + d   | Delete the Word after the cursor                                                  |
| Ctrl + d  | Delete character under the cursor                                                 |
| Ctrl + h  | Delete character before the cursor (backspace)                                    |
| Ctrl + w  | Cut the Word before the cursor to the clipboard                                   |
| Ctrl + k  | Cut the Line after the cursor to the clipboard                                    |
| Alt + t   | Swap current word with previous                                                   |
| Ctrl + t  | Swap the last two characters before the cursor (typo)                             |
| Esc + t   | Swap the last two words before the cursor.                                        |
| Ctrl + y  | Paste the last thing to be cut (yank)                                             |
| Alt + u   | UPPER capitalize every character from the cursor to the end of the current word.  |
| Alt + l   | Lower the case of every character from the cursor to the end of the current word. |
| Alt + c   | Capitalize the character under the cursor and move to the end of the word.        |
| Alt + r   | Cancel the changes and put back the line as it was in the history (revert)        |
| Сtrl + _  | Undo                                                                              |

### History

| Command  | Description                                                                                        |
|----------|----------------------------------------------------------------------------------------------------|
| Ctrl + r | Recall the last command including the specified character(s)(equivalent to : vim ~/.bash_history). |
| Ctrl + p | Previous command in history (i.e. walk back through the command history)                           |
| Ctrl + n | Next command in history (i.e. walk forward through the command history)                            |
| Ctrl + s | Go back to the next most recent command.                                                           |
| Ctrl + o | Execute the command found via Ctrl+r or Ctrl+s                                                     |
| Ctrl + g | Escape from history searching mode                                                                 |
| Alt + .  | Use the last word of the previous command                                                          |

### Process control

### Bang(!)
Bash also has some handy features that use the ! (bang) to allow you to do some funky stuff with bash commands.

| Command | Description                                                                                                                          |
|---------|--------------------------------------------------------------------------------------------------------------------------------------|
| !!      | run last command                                                                                                                     |
| !blah   | run the most recent command that starts with ‘blah’ (e.g. !ls)                                                                       |
| !blah:p | print out the command that !blah would run (also adds it as the latest command in the command history)                               |
| !$      | the last word of the previous command (same as Alt + .)                                                                              |
| !$:p    | print out the word that !$ would substitute                                                                                          |
| !*      | the previous command except for the last word (e.g. if you type ‘find some_file.txt /‘, then !* would give you ‘find some_file.txt‘) |
| !*:p    | print out what !* would substitute                                                                                                   |

### Bash special variables

| Variable        | Description                                                                                                  |
|-----------------|--------------------------------------------------------------------------------------------------------------|
| $1, $2, $3, ... | Positional parameters                                                                                        |
| $0              | Name of the shell or shell script.                                                                           |
| "$@"            | Array-like construct of all positional parameters, {$1, $2, $3 ...}                                          |
| "$*"            | IFS expansion of all positional parameters, $1 $2 $3 ....                                                    |
| $#              | Number of positional parameters.                                                                             |
| $-              | Current options set for the shell.                                                                           |
| $$              | PID of the current shell (not subshell).                                                                     |
| $_              | Most recent parameter (or the abs path of the command to start the current shell immediately after startup). |
| $IFS            | (input) field separator.                                                                                     |
| $?              | Most recent foreground pipeline exit status.                                                                 |
| $!              | PID of the most recent background command.                                                                   |

* [Special Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html)
* [Shell Variables](https://www.gnu.org/software/bash/manual/html_node/Shell-Variables.html)
* [Parameter and Variable Index](https://www.gnu.org/software/bash/manual/html_node/Variable-Index.html)

## Recent links
* [Bash Shortcuts For Maximum Productivity](http://www.skorks.com/2009/09/bash-shortcuts-for-maximum-productivity/)
* [Syntax Bashkeyboard](http://ss64.com/osx/syntax-bashkeyboard.html)
