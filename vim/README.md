# Vim cheatsheet


| Action                | command                           | Details |
| :---                | :---:                                | ---:|
| Insert text | i | Insert text in the current position |
| Insert text | shift+A | Insert text at the end of the current line |
| Insert text | I | Insert text at the beginning of current line |
| Move cursor | w | Moves the cursor forward jumping words |
| Move cursor | b | Moves the cursor backwards jumping words |
| Move cursor | ^ | Moves the cursor to the beginning of current line |
| Move cursor | $ | Moves the cursor to the end of current line |
| Move cursor | gg | Moves the cursor to the beginning of the document |
| Move cursor | G  | Moves the cursor to the end of the document |
| Search and replace  | :s/foo/bar/ | Seaches word "foo" and replaces with "bar": first occurrence in current line |
| Search and replace  | :s/foo/bar/g | Seaches word "foo" and replaces with "bar": all occurrences in current line |
| Search and replace  | :%s/foo/bar/g | Seaches word "foo" and replaces with "bar": all occurrence in all document |
| Search and replace  | :%s/foo/bar/gc | Seaches word "foo" and replaces with "bar": all occurrence in all document asking you first in each case |
| Repeat  | . | Repeats the last action |
| Run shell command   | :! command | Allows you to run a shell command without closing vim first, e.g., :! ls -lrt |


