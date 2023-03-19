---
title: "Developer productivity 2023"
date: 2023-03-18T09:35:20+02:00
draft: true
toc: true
---

Highly inspired by [ThePrimeagen](https://github.com/theprimeagen).

## Windows

### Navigation between projects

Using the new windows terminal, navigate to a project in a new tab and open 
visual studio with the .sln file that can be found in that specific folder.

Demo of this workflow in the gif below.

![sessionizer](posts/developer-productivity-2023/images/sessionizer.gif "sessionizer")

#### Step by step 

Start with an empty terminal. 

![sessionizer1](posts/developer-productivity-2023/images/sessionizer1.png "sessionizer1")

Press `<C-f>` (configurable, we'll get to that later) to get to the fuzzy search (fzf).

![sessionizer2](posts/developer-productivity-2023/images/sessionizer2.png "sessionizer2")

Fuzzy search the folder/project you want to navigate to. In the example below 
I search for tes and fzf will make a best guess which project you want to select.
If it's not the chosen one, either keep typing or use the arrow keys to navigate.

![sessionizer3](posts/developer-productivity-2023/images/sessionizer3.png "sessionizer3")

Select the chosen one with enter. A new tab will open in the windows terminal
in your selected folder/project.

![sessionizer4](posts/developer-productivity-2023/images/sessionizer4.png "sessionizer4")

Use the alias `vs` to open visual studio with the .sln-file in that directory.

![vs1](posts/developer-productivity-2023/images/vs1.png "vs1")


## Linux/MacOS

### Navigation between projects
tmux-sessionizer
![sessionizer](posts/developer-productivity-2023/images/tmux-sessionizer.gif "tmux-sessionizer")
