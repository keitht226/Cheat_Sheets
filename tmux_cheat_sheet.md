<center>
# Tmux Cheat Sheet
</center>

Prefix key is set in .tmux.config  
**Anything in _italics_ is unique to my .tmux.config and not default behavior**

## Contents
**[Basics](Basics)**  
**[Sessions (workspaces)](Sessions (workspaces))**  
**[Window Shortcuts (tabs)](Window Shortcuts (tabs))**  
**[Pane Shortcuts (splits)](Pane Shortcuts (splits))**  
**[Misc](Misc)**  

## Basics
| Command | Effect |
| --- | --- |
| tmux | start new |
| tmux new -s session_name | start new with session name |
| tmux ls | list sessions |
| tmux kill-session -t session_name | kill session |

## Sessions (workspaces)
| Key Sequence <br>(starting with prefix) | Effect |
| --- | --- |
| :new<\CR> | new sessions |
| s | list sessions |
| $ | rename current session |
| ( | previous session |
| ) | next session |

## Window Shortcuts (tabs)
| Key Sequence <br>(starting with prefix) | Effect |
| --- | --- |
| c | create window |
| w | list windows |
| n | next window |
| p | previous window |
| f | find window |
| , | rename window |
| & or type 'exit' | kill window |

## Pane Shortcuts (splits)
| Key Sequence <br>(starting with prefix) | Effect |
| --- | --- |
| % *or V* | vertical split |
| " *or S* | horizontal split |
| *M (meta) + hjkl (vim directions)* | change pane focus |
| o | swap panes |
| q | show pane numbers. Hit the number to jump to a pane |
| x | kill pane |
| + | break pane into window |
| - | restore pane from window |
| space | toggle between layouts |
| { | move the current pane left |
| } | move the current pane right |
| z | toggle pane zoom |

## Misc
* With my .tmux.config, the mouse is enable and can be used to resize panes. <br>
However, with an older version of tmux, this may not be possible. A command can be used.
` <prefix> : resize-pane -t <pane_id> -DULR <number of cells>`   
> DULR : down up left right, pick one
* Copy mode:   
    * Enter/Exit copy mode: `<prefix> [ or ]`
    * *Use vim commands to select stuff to copy, then press 'y' to copy (like vim)*
    * *Paste: `<prefix> p`*
* *Reload config file: `<prefix> r`*
