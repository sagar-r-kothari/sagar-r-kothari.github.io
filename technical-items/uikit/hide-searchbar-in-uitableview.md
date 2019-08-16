---
description: Code snippet to hide UISearchBar appearing on top of the UITableView
---

# Hide SearchBar in UITableView

If you've a searchbar in your TableView & you wish to hide it when tableview is appeared on screen, use following code snippet to hide it. It will appear only when user will pull down.

```swift
self.tableView.scrollToRow(
    at: IndexPath(row: 0, section: 0), 
    at: .top, 
    animated: true
)
```

