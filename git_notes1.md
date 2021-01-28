# Introduction

I put together some git notes that I should, but never will, put into a Medium blog post.  Most of you know all of this but maybe it will help someone do something they had trouble with.  Most of the command sequences can be simplified but I use these because they are fairly explicit, simple and I can remember them.
 
# GITNOTES

Make git useful and show some common sequences.  I’ve been consistently working on multiple branches of a repository at more or less the same time.  Some of these notes help make switching branches fairly simple and painless.  I’ve also got opinions about clean commit history. Some of these sequences help avoid adding extra commit messages. One important terminology change is rebasing instead of pulling from master. 

These notes are very opinionated. git pull —rebase and  git rebase are used a lot and can mess things up.  Please don’t rebase commits that are already in the remote master branch.


# .gitconfig


Add your favorite aliases for commands

[alias]
                co = checkout

Add your favorite mappings to access gitlab

[url "git@gitlab.com:"]
                insteadof = https://gitlab.com/
 

# Set up environment variables and use zsh

If you ever have to use a command line interface to git, this is the minimum that should be set up.
Zsh will be the default mac shell soon and is much friendlier than bash for developers.
Your prompt will show the repository branch with these changes in place. Zsh can be infinitely customized. 
This set up also helps with Goland by getting the right environment variables in place.

* Use zsh as your default shell :-). Run “chsh -s /usr/bin/zsh”
* Use oh-my-zsh - https://ohmyz.sh/#install
* Update ~/.zshrc with these at a minimum
* export ZSH="/Users/Mark.Gibbons/.oh-my-zsh"
* plugins=(git)
* source $ZSH/oh-my-zsh.sh
* GITLAB_TOKEN=<your token>
* GITLAB_ID=mgibbonsid
 

# New branch - from current remote origin master

This sequence will update your local copy of master and create a new branch without adding any extra unneeded commits to the repository.

* git co master
* git pull —rebase origin master
* git co -b AWEB-#NEW# 
 

# Working on a branch with uncommitted changes

## Rebase with origin master without committing. 
This sequence lets you keep updating without adding extraneous commit loops.

* git stash
* git pull —rebase origin master   # you can set the default pull option to rebase, you should know a lot about rebase first
* git stash apply
 
# Make a change on branch b while I’m working on branch a without committing to branch a

* a: git stash                                # Start on branch a
* a: git co (-b to create) b
* b: Make changes on branch b # Switch to branch b
* b: git commit -am “commit b changes”
* b: git push origin b
* b: git co a
* a: git stash apply                      # Switch back to branch a
* a: continue knitting  
 

# Working on a branch with committed changes

## Rebase with origin master. 

Avoid adding an extra merge master to local commit. You’ll get a much cleaner commit history. 

* git pull —rebase origin master
* Fix any conflicts - loop until all commits from master have been added
* git add  [ list of fixed conflicted files]
* git rebase —continue
 

# Working on a branch with multiple commits for the same feature

I usually do this before every merge request. Commits and merge requests for a single feature should generally be a single commit. You can fix your commits with this procedure.  If you work somewhere else or contribute to open source clean commits may be expected.   10 commits in an MR saying “fixed tpyo” with the consistent spelling error drives people nuts.

* git pull —rebase origin master
* git log - find the commit before the commits for this feature call this <base>. It will usually be marked (HEAD -> AWEB-XXXXX, origin/master, origin/HEAD)
* git rebase -i <base>
* Enter the interactive rebase dialog
*     Change the commits you want to combine to s(quash)
*     Leave the commits you want keep as p(ick)
*     Many other options, your risk
*     Exit the squash dialog
* Enter the commit message edit dialog
*     Edit the commit messages. Can combine, delete, edit the messages as needed
 

# Fix the last commit message

* git commit —amend
 

# See what changed

* git log -p   to get change details by file and commit

# links
merge vs rebase - https://medium.com/datadriveninvestor/git-rebase-vs-merge-cc5199edd77c 
oh my zsh plugins - https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins
 
