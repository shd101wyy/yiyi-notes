---
note:
    createdAt: 2020-05-22T14:10:37.568Z
    modifiedAt: 2020-06-01T14:30:08.361Z
    tags: [小工具]
    id: ""
---
# Tmux Cheat Sheet & Quick Reference

[Source](https://tmuxcheatsheet.com/ "Permalink to Tmux Cheat Sheet & Quick Reference")
![](https://i.loli.net/2020/06/01/9oDuXtIcNkVMvWJ.png)  



## Sessions

$ tmux

$ tmux new

$ tmux new-session

: new

Start a new session

$ tmux new -s mysession

: new -s mysession

Start a new session with the name *mysession*

$ tmux kill-ses -t mysession

$ tmux kill-session -t mysession

kill/delete session *mysession*

$ tmux kill-session -a

kill/delete all sessions but the current

$ tmux kill-session -a -t mysession

kill/delete all sessions but *mysession*

Ctrl + b  $

Rename session

Ctrl + b  d

Detach from session

: attach -d

Detach others on the session (Maximize window by detach other clients)

$ tmux ls

$ tmux list-sessions

Ctrl + b  s

Show all sessions

$ tmux a

$ tmux at

$ tmux attach

$ tmux attach-session

Attach to last session

$ tmux a -t mysession

$ tmux at -t mysession

$ tmux attach -t mysession

$ tmux attach-session -t mysession

Attach to a session with the name *mysession*

Ctrl + b  (

Move to previous session

Ctrl + b  )

Move to next session

## Windows

$ tmux new -s mysession -n mywindow

start a new session with the name *mysession* and window *mywindow*

Ctrl + b  c

Create window

Ctrl + b  ,

Rename current window

Ctrl + b  &

Close current window

Ctrl + b  p

Previous window

Ctrl + b  n

Next window

Ctrl + b  0 ... 9

Switch/select window by number

: swap-window -s 2 -t 1

Reorder window, swap window number 2(src) and 1(dst)

: swap-window -t -1

Move current window to the left by one position

## Panes

Ctrl + b  ;

Toggle last active pane

Ctrl + b  %

Split pane vertically

Ctrl + b  "

Split pane horizontally

Ctrl + b  {

Move the current pane left

Ctrl + b  }

Move the current pane right

Ctrl + b  

Ctrl + b  

Ctrl + b  

Ctrl + b  

Switch to pane to the direction

: setw synchronize-panes

Toggle synchronize-panes(send command to all panes)

Ctrl + b  Spacebar

Toggle between pane layouts

Ctrl + b  o

Switch to next pane

Ctrl + b  q

Show pane numbers

Ctrl + b q 0 ... 9

Switch/select pane by number

Ctrl + b  z

Toggle pane zoom

Ctrl + b  !

Convert pane into a window

Ctrl + b  + 

Ctrl + b  Ctrl + 

Ctrl + b  + 

Ctrl + b  Ctrl + 

Resize current pane height(holding second key is optional)

Ctrl + b  + 

Ctrl + b  Ctrl + 

Ctrl + b  + 

Ctrl + b  Ctrl + 

Resize current pane width(holding second key is optional)

Ctrl + b  x

Close current pane

## Copy Mode

: setw -g mode-keys vi

use vi keys in buffer

Ctrl + b  [

Enter copy mode

Ctrl + b  PgUp

Enter copy mode and scroll one page up

q

Quit mode

g

Go to top line

G

Go to bottom line

Scroll up

Scroll down

h

Move cursor left

j

Move cursor down

k

Move cursor up

l

Move cursor right

w

Move cursor forward one word at a time

b

Move cursor backward one word at a time

/

Search forward

?

Search backward

n

Next keyword occurance

N

Previous keyword occurance

Spacebar

Start selection

Esc

Clear selection

Enter

Copy selection

Ctrl + b  ]

Paste contents of buffer\_0

: show-buffer

display buffer\_0 contents

: capture-pane

copy entire visible contents of pane to a buffer

: list-buffers

Show all buffers

: choose-buffer

Show all buffers and paste selected

: save-buffer buf.txt

Save buffer contents to *buf.txt*

: delete-buffer -b 1

delete buffer\_1

## Misc

Ctrl + b  :

Enter command mode

: set -g OPTION

Set OPTION for all sessions

: setw -g OPTION

Set OPTION for all windows

## Help

$ tmux info

Show every session, window, pane, etc...

Ctrl + b  ?

Show shortcuts
