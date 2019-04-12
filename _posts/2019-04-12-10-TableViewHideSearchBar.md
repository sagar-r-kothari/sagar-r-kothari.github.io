---
layout: post
title:  "Hide SearchBar in UITableView"
date:   2019-04-12 16:00:00 +0530
categories: Swift UIKit
---

If you've a searchbar in your TableView & you wish to hide it when tableview is appeared on screen, use following code snippet to hide it.
It will appear only when user will pull down.

```swift
self.tableView.scrollToRow(at: IndexPath(row: 0, section: 0), at: .top, animated: true)
```