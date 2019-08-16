---
description: which I use very frequently
---

# Git commands on zsh

### _ggpush_

is equivalent to

```text
git push origin currentBranch
```

### _ggpull_

is equivalent to

```text
git pull origin currentBranch
```

### _ggpull someBranch_

is equivalent to

```text
git pull origin someBranch
```

It pulls `someBranch` to and merges to local `currentBranch`.

### _gco someBranch_

is equivalent to

```text
git checkout someBranch
```

### _gco ._

is equivalent to

```text
git checkout .
```

* It checkouts latest commit in current branch.
* It is used to undo/remove your local changes.
* Warning: all your local changes would be lost.

### _gd_

is equivalent to

```text
git diff
```

* To show current changes in terminal.

### _gd someRevNo / gd someBranch_

* To show difference between current head and given reference

### _gcb someNewBranch_

is equivalent to

```text
git checkout branch someNewBranch
```

* I use it to create a new branch

### _gcl someRepoURL_

is equivalent to

```text
git clone someRepoURL
```

* To clone repository locally

### _gaa_

is equivalent to

```text
git add --all
```

### _gcmsg someText_

is equivalent to

```text
git commit -m someText
```

### _glog_

is equivalent to

```text
git log --oneline --decorate --graph
```

* To see logs of commit with lines

### _gss_

is equivalent to

```text
git status
```

* To see list of files with changes

### _gcam someMessage_

is equivalent to

```text
git commit -a -m
```

* Add all changes and commit with a message

