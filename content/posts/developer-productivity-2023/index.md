---
title: "Developer productivity 2023"
date: 2023-03-25T09:35:20+02:00
draft: false
---

My [dotfiles](https://github.com/jawee/.dotfiles/tree/master) contains everything below, the windows folder differs greatly because it's windows specific but the rest of the folders are pretty much just copies of eachother with some slight variations.

## Navigation between projects
When you're working with multiple projects, having a fast and easy way to switch between projects is essential for having a smooth and productive workflow.
I've been using [ThePrimeagen](https://github.com/theprimeagen)'s `tmux-sessionizer` for a couple of years on MacOS and Linux, which makes it super nice to switch between projects through just a keyboard shortcut and the tmux-session is exactly the same as it was when you left it.

On Windows I haven't found a good way of replicating this workflow, since there isn't really a `tmux` alternative for powershell. Instead I have opted for a slightly subpar solution using the new windows terminal, that opens a new tab for the chosen project. 

### Windows

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


### Linux/MacOS

#### Navigation between projects
`tmux-sessionizer` in action below. I have it bound to <C-f>, such that I have the same shortcut regardless of development environment I'm in.

What makes `tmux-sessionizer` so nice, is that if you open your editor of choice (neovim) and a couple of other windows in that session, even if you navigate to another project, when you navigate back you're right back where you started.

![sessionizer](posts/developer-productivity-2023/images/tmux-sessionizer.gif "tmux-sessionizer")

## Other tools

### Git worktree
I've been using [git worktrees](https://git-scm.com/docs/git-worktree) for many years, which helps out immensely when you want to have multiple checkouts of the same repository. I have to jump between feature-branches, fix-branches and master multiple times a day to fix something, see why something isn't working and to keep the projects I work on going by implementing new featues. Git worktrees makes this nice, instead of juggling `git checkout <branchname>` and `git stash`, I clone all my repositories as a bare repository `git clone --bare <repo-url> <repo-name>`. In that bare repository I can then add multiple worktrees by running the command `git worktree add <branch-name>`.

In the example below I have a bare clone of my project [twitch-recorder](https://github.com/jawee/twitch-recorder), where I have added the worktree `master` that, obviously, tracks the master branch, but I have also added the worktree `feat-some-feature` that is a feature branch. So now I have two worktrees, linked against master, no need to have multiple clones of the repository and each of these can have their own working state.

![bare-repo-example](posts/developer-productivity-2023/images/bare-repo-example.png "bare repository")

Because I'm used to the branch naming standard `(feature/fix/bug)/name-of-branch`, this doesn't play well with the basic `git worktree add <branch-name>` command, because you can then only have one feature-branch at once. Hence I usually name my worktrees `(feat/fix/bug)-name-of-branch` but also tell that the worktree should track the remote branch `(feature/fix/bug)/name-of-branch`. This is possible to do by executing the following command, `git worktree add feat-name-of-branch -b feature/name-of-branch`, to create a feature branch.

Since I usually do this quite often, I wrote a tool called [new-worktree](https://github.com/jawee/new-worktree) to help me out.

#### New-worktree
I have the tool aliased to `nwt` because I'm lazy. This is the initial prompt, where you can choose if you want to add a feature, fix or bug branch. Select using the arrow keys and press enter to select.

![nwt-1](posts/developer-productivity-2023/images/nwt-1.png "nwt-1")

Write the name of the branch and press enter to continue.

![nwt-2](posts/developer-productivity-2023/images/nwt-2.png "nwt-2")

A new worktree has been added, in this case the `feat-some-new-feature` that automatically tracks the remote branch `feature/some-new-feature`.

![nwt-3](posts/developer-productivity-2023/images/nwt-3.png "nwt-3")