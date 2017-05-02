# GIT Cheat Sheet

| Command | Usage |
| --- | --- |
| git init | create new local repository |
| git remote add origin \<server\> | connect new repository to remote server |
| git clone /path/to/repository | create a working copy of a repository |
| git status | List changed files and files that need to be added or committed |
| git mv \<filename> \<new path> | rename a file or move a file |
| git add \<filename\> <br>git add * | add files to be staged |
| git commit -a -m "description of changes" | commit staged files |
| git push origin master | update remote repository |
| git pull origin master | update local copy with changes from remote repository |
| git checkout -b \<branch name\> | create a new branch and switch to it |
| git checkout \<branch name> | switch from one branch to another |
| git branch | list all branches and show which one is checked out |
| git branch -d \<branch name> | delete branch |
| git push origin \<branch name> | push branch to remote and make accesible to others |
| git push --all origin | push all branches |
| git merge \<branch name> | merge different branch with currently checked out branch |
| git diff | view all merge conflicts |
| git diff \<source branch> \<target branch> | preview changes before mergin |
| git tag 1.0.0 \<commitID> | tag to mark a significant version |
| git log | get the commitID |
| git push --tags origin | push all tags |
| git checkout -- \<filename> | undo changes |
| git fetch origin && git reset --hard origin/master | ditch all changes |
| git grep | search for stuff like in linux terminal |
| git blame | shows copy of file annotated with changes and who committed |

Fugitive
| Command | Usage |
| --- | --- |
| :Git \<command> | run any git command |
| :Gwrite | add current file and close buffers used to resolve conflicts |
| :Gread | revert changes to last commit |
| :Gremove | wipes vim buffer and removes file from git |
| :Gmove target_path (relative to current file locaiton) | move current file |
| :Gcommit | Opens split buffer and can add message for commit |
| :Gblame | vertical split scroll binds them with blam annotations |
| :Gdiff | show staged changes in split windows. **Working copy is always on right.** |
| :Gstatus | opens interactive git status in split window. <br><br> Pressing '-' on unadded files will call *git add* on them. <br><br>^n or ^p jump around the status window, skipping comments. <br><br> If the cursor is on a file that is already staged, pressing '-' will reset those files (un-add them).<br><br> pressing Enter on a file will open the file in a new window, making it easy to review a file before committing it. <br><br> Shift+C in status window is a shortcut for :Gcommit|
| :Git add . | add all unstaged files that appear in :Gstatus window |
| :diffget :do| grabs changes from working copy and adds them to original, or indexed, copy |
| :diffput :dp| puts added changes in working copy to original, or indexed, copy | 
| :ls | lists buffer ids |
| :diffupdate | diffgets and diffputs can mess with the diff highlighting. This fixes it |
| :only | close all splits but current buffer |

Running :Gdiff on a merge conflicted file opens up 3 split windows. Left is the active branch, mid is the working copy, right is the branch mergin in. diffget and diffput get an argument in this case. The argument is the ID of the buffer you want to use. [c and ]c jump between conflicted sections. Shorthand :do doesn't work, but :dp does. 