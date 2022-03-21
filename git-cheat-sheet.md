# git cheat sheet

Markdown hints: https://www.markdownguide.org/basic-syntax

## pushing a branch
`git push -u origin HEAD`

## set the tracking branch at creation time
`git checkout --track origin/dev`

## changing the commit message before push
`git commit --amend -m "New commit message."`

## changing the commit message before push
1. `git commit --amend -m "New commit message."`
2. `git push --force`

## resetting a workspace
1. `git reset --hard`
2. `git clean -xdf`

## undo the last commit but keeping the changes
`git reset --soft HEAD~1`

## undo the last commit without keeping the changes
`git reset --hard HEAD~1`

## moving changes to a different branch
1. `git checkout existingbranch`
2. `git merge master`
3. `git checkout master`
4. `git reset --hard HEAD~3` # Go back 3 commits. You *will* lose uncommitted work.
5. `git checkout existingbranch`

## `git log` for a graph
`git log --oneline --graph --decorate`

## `git log` for a specific file
`git log -- filename.ext`

## `git log` for a specific file with rename tracking
`git log --follow -- filename.ext`

## move changes to a new branch easily
1. add files to a stash 
2. `git stash branch newbranchname`

## remove all remote branches with no local counterpart
`git push --prune`

## `git log` alias
`git config --global alias.hist "log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short --decorate"`

**BE CAREFUL...this could be less than pleasant if executed in the wrong context**

# References
1. https://devconnected.com/how-to-set-upstream-branch-on-git/
2. https://linuxize.com/post/change-git-commit-message/
