<center>
# Vim Cheat Sheet
</center>

__I recommend using _ctrl [_ instead of ESC to go into normal mode__

## Contents
**[Movement](#movement)**  
**[Editing](#editing)**  
**[Find/Replace](#find-replace)**  
**[Tabs/Windows/Buffers](#tabs-windows-buffers)**  
**[Folding/Marks](#folding-marks)**  
**[Other Notes](#other-notes)**  

## Movement
| Description | Vim Command | Notes |
| --- | --- | --- |
| up,down,left,right | kjhl | 
| jump to the beginning of the next word | w | 
| jump to the beginning of the previous word | b |
| jump to the next occurrence of _char_ | f _char_ |
| jump to the previous occurrence of _char_ | F _char_ |
| page up | ctrl u |
| page down | ctrl d |
| jump to end of line | $ |
| jump to beginning of line | 0 |
| jump to line _line nu_ | : _line nu_ or gg _line number_ | 
| jump to begging of file | gg | 
| jump to end of file | G |
| jump to declaration of function/variable within current file | gd |
| return to original position from any jump | ctrl o |

## Editing
| Description | Vim Command | Notes |
| --- | --- | --- |
| Change Colorscheme | :colorscheme _colorSchemeName_ |
| List available colorschemes | :colorscheme ctrl d |
| repeat last command | . |
| yank (copy) | y | can be combined with many things |
| paste | p |
| visual mode | v |
| visual block mode | V | _lineNu_ gg is useful in combination with this|
| Easy Block Commenting in code | __0__ (to jump to beginning of line) __ctrl v__ move up or down __I__ language specific comment characters __ctrl [__ move cursor anywhere to take effect | of course, comments can be deleted in similar fashion |
| delete and then enter insert mode (change) | c | useful combos: ce c$ c0 ci( ci[ ci{ ci" |
| inside _char_ | i _char_ | used in combo with d,c,y,etc. yiw is useful for copying word under cursor regardless of position. |
| Insert at beginning of line | I |
| Insert at end of line | A |
| Delete Character under cursor | x | useful with number combos (3x to delete next 3 characters) |
| Replace | r | keeps you in normal mode after typing the character to replace with |
| Replace mode | R | like hitting _insert_ in most text editors |
| Insert blank line underneath current line and enter insert mode | o | 
| Insert blank line above current line and enter insert mode | O |
| adjust tab indent of current line | >> or << | can shift entire blocks using visual mode. Use . to repeat. |
| record commands for repeated use to _key_ | q _key_ _any number of commands to be recorded_ q | use @ _key_ to perform recorded commands. To perform the recorded action repeatedly, type a number before @. For example, 10@a will do all the actions I recorded to 'a' ten times. |

## Find-Replace
| Description | Vim Command | Notes |
| --- | --- | --- |
| Search forward for _string_ | /_string_ |
| Search backwards for _string_ | ?_string_ |
| jump to next result | n |
| jump to previous result | N |
| Find and Replace all of _stringA_ with _stringB_ | :%s/_stringA_/_stringB_/g |
| Find and Replace all of _stringA_ with _stringB_ and ask for confirmation for each result | :%s/_stringA_/_stringB_/gc |
| Remove highlight after search | :nohl |
| Find all occurrences of word under cursor | * |

## Tabs-Windows-Buffers
| Description | Vim Command | Notes |
| --- | --- | --- |
| split window horizontally | :split |
| split window vertically | :vsplit or :vs |
| switch window focus | ctrl w _direction_ |
| toggle window focus between two windows | ctrl w w |
| make new tabe | :tabe |
| open _FilePath_ in a new tab | :tabe _FilePath_ |
| open _FilePath_ in current buffer | :e _FilePath_ |
| go to next tab | gt |
| go to previous tab | gT |
| move tab to position # (starting at 0) | :tabm # |
| save all tabs/windows/buffers | :wa |
| quit all tabs/windows/buffers | :qa |
| list all open buffers | :buffers |
| switch window focus to buffer number as displayed with :buffers | b# |

## Folding-Marks
| Description | Vim Command | Notes |
| --- | --- | --- |
| Fold lines | Highlight block to be folded with V, zf |
| Open fold | zo |
| close fold | zc | 
| open all folds | zR |
| close all folds | zC |
| set marks | m _key_ | only works with a-z, 1-0. Upper case letters are saved between sessions even without saving the file. |
| jump to line of mark | '_mark_ |
| jump to line and column of mark | \`_mark_ |
| list all marks | :marks |
| delete a mark | :delmarks _mark_ |
| wipe buffer, including all marks | :bw |


## Other Notes
* To open a file from the command line in an existing instance of gvim, use the option --remote-tab-silent. This doesn't play well with multiple instances of gvim however. 
* To open multiple files in individual tabs from the command line: gvim -p file1 file2 file3 ...
* Doubling a command often affects the whole line. dd will delete a whole line. yy will copy a whole line. etc. 
* In command mode, % is shorthand for the path to the current file
