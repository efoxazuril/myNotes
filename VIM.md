VIM

Normal mode: ESC

In normal mode:

Start of Line: 0, gg
End of Line: $, G
Move between (,{,[ : %

Next word: w
End of the word: 

Find symbol: f<symbol>
Next: *
Previous: #
Search: /<word_to_search> ,  `n` for next, `N` for previous
Search in top: ?<word_to_search>

New line: `o`
New line before: `O`

Delete under cursor: `x` 
Replace under cursor: `r`

Movement:
    go to prev edit place: `gi`
Splits:
    Split window horizontally:
    Split window vertically: 
Replace:
    char: `r`
    word: `cw`

Delete:
	word: `dw`
	line: `dd`
	paste deleted: `p`
Inside:
    Universal <command>i<char>
    
    Delete something inside:
        brackets ( : di(
        brackets [ : di[
        quotes   " : di"
        etc.
 
    Replace something inside:
        brackets { : ci{
        quotes   ' : ci'


Repeat last command: `.`

Visual mode: `v`

UNDO: `u`
REDO: `control+R`

Duplicate line: `Shift+y` to copy entire line, `p` for paste

Go to line: <line number> G
Go to paring brackets: `%`
Comment:

Install : https://vimawesome.com/plugin/tcomment

gcc - comment current line

Install: 

Help: `:help`


Insert Mode: `i`

Commands:
    Open terminal: :ter
    New tab: :tabnew
    Close tab: :tabc
        Go to next tab: gt
        Go to prev. tab: gT
Config<in .vimrc>:
    Set line numbers: set nu
    Set relative line numbers: set rnu
    
