---
layout: post
title:  "Tmux"
date:   2020-10-29 19:56:57 +0800
categories: jekyll update
---
# Tmux

`Tmux` has a hierachy of objects:
* Sessions - an independent workspace with one or more windows
  * `tmux` starts a new session
  * `tmux new -s NAME` starts it with that name
  * `tmux ls` lists the current sessions
  * Within `tmux`, `<C-b> d` detaches the current session
  * `tmux a [-t NAME]` attach the specified session
* Windows - equivalent to tabs in editors or browsers
  * `<C-b> c` creates a new window. To close it, type `<C-d>`
  * `<C-b> N` Go to the N-th window. 
  * `<C-b> p` Go to the previous window
  * `<C-b> n` Go to the next window
  * `<C-b> ,` Rename the current window
  * `<C-b> w` List windows in current session
* Panes - equivalent to multiple shells in the same visual display
  * `<C-b> "` split the current pane horizontally
  * `<C-b> %` split the current pane verticallly 
  * `<C-b> <direction>` move to the pane in the specified direction
  * `<C-b> z` toggle zoom for the current pane
  * `<C-b> [` 
  * `<C-b> <space>`