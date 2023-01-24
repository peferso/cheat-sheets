# Vim cheatsheet


| Action                | command                           | Details |
| :---                | :---:                                | ---:|
| Delete and insert | cc | Deletes current line and changes to insert mode |
| Delete and insert | C  | Deletes from current position to end of line and changes to insert mode |
| Delete and insert | cw | Deletes current word and changes to insert mode |
| Indentation | == | Autoindents current line |
| Indentation | =G | Autoindents whole file if cursor is in line 1 |
| Insert text | i | Insert text in the current position |
| Insert text | shift+A | Insert text at the end of the current line |
| Insert text | I | Insert text at the beginning of current line |
| Line numbers | :set number | Shows line numbers |
| Move cursor | w | Moves the cursor forward jumping words |
| Move cursor | b | Moves the cursor backwards jumping words |
| Move cursor | ^ | Moves the cursor to the beginning of current line |
| Move cursor | $ | Moves the cursor to the end of current line |
| Move cursor | gg | Moves the cursor to the beginning of the document |
| Move cursor | G  | Moves the cursor to the end of the document |
| Move cursor | Ctrl + w [hjkl] | Moves the cursor between split panes |
| Open file | :vsp filename | Makes a vertical split and opens file `filename` |
| Open file | :sp filename | Makes a horizontal split and opens file `filename` |
| Search and replace  | :s/foo/bar/ | Seaches word "foo" and replaces with "bar": first occurrence in current line |
| Search and replace  | :s/foo/bar/g | Seaches word "foo" and replaces with "bar": all occurrences in current line |
| Search and replace  | :%s/foo/bar/g | Seaches word "foo" and replaces with "bar": all occurrence in all document |
| Search and replace  | :%s/foo/bar/gc | Seaches word "foo" and replaces with "bar": all occurrence in all document asking you first in each case |
| Repeat  | . | Repeats the last action |
| Run shell command   | :! command | Allows you to run a shell command without closing vim first, e.g., :! ls -lrt |

# Default settings

Default vim settings are stored in a file called `.vimrc` which usually is contained in the home directory.

A possible set of useful settings inside the `.vimrc` file could be:
```
set hlsearch # highlight search results
set incsearch # incremental highlight search results
set ignorecase # ignore case when searching
set relativenumber # show relative line numbers
set number # to show current line number
inoremap jj <Esc> # to exit insert mode typing "jj" instead of Esc key
```
these settings can be activated temporarily inside the document just writing the commands with a semicolon first, e.g., `:set relativenumber`, `:set norelativenumber`, `:set number`, `:set nonumber`...
