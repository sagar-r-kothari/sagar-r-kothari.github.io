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