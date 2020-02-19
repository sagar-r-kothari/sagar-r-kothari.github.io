---
layout: post
title: "Git - Undo last commit"
date: 2020-02-19 07:20:00 +0530
categories: git
---

```git
$ git commit -m "Something terribly misguided"             # (1)
$ git reset HEAD~                                          # (2)
<< edit files as necessary >>                              # (3)
$ git add ...                                              # (4)
$ git commit -c ORIG_HEAD                                  # (5)
```

1. Your last commit
2. Sencond step above, illustrates how to move your git-head one commit back.
3. Make changes - Remove changes which you didn't want.
4. Add your new changes.
5. Commit those changes ✅👍 - You're all set.