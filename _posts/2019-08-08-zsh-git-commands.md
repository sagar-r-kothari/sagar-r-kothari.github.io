---
layout: post
title:  "git commands on zsh which I use Most commonly"
date:   2019-08-08 17:00:00 +0530
categories: zsh
---

## *ggpush*

is equivalent to 

```
git push origin currentBranch
```

## *ggpull*

is equivalent to 

```
git pull origin currentBranch
```

## *ggpull someBranch*

is equivalent to

```
git pull origin someBranch
```

It pulls `someBranch` to and merges to local `currentBranch`.

## *gco someBranch*

is equivalent to 

```
git checkout someBranch
```

## *gco .*

is equivalent to

```
git checkout .
```
- It checkouts latest commit in current branch.
- It is used to undo/remove your local changes.
- Warning: all your local changes would be lost.

## *gd*

is equivalent to

```
git diff
```

- To show current changes in terminal.

## *gd someRevNo / gd someBranch*

- To show difference between current head and given reference

## *gcb someNewBranch*

is equivalent to 

```
git checkout branch someNewBranch
```

- I use it to create a new branch

## *gcl someRepoURL*

is equivalent to

```
git clone someRepoURL
```

- To clone repository locally

## *gaa*

is equivalent to

```
git add --all
```

## *gcmsg someText*

is equivalent to 

```
git commit -m someText
```

## *glog*

is equivalent to

```
git log --oneline --decorate --graph
```

- To see logs of commit with lines

## *gss*

is equivalent to

```
git status
```

- To see list of files with changes

## *gcam someMessage*

is equivalent to

```
git commit -a -m
```

- Add all changes and commit with a message

