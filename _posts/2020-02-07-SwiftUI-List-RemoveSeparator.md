---
layout: post
title: "SwiftUI - List / UITableView Remove Separator"
date: 2020-02-07 06:20:00 +0530
categories: SwiftUI List
---

```swift
import SwiftUI

struct MyListView: View {
    var body: some View {
        List {
            Text("Item 1")
            Text("Item 2")
            Text("Item 3")
        }.onAppear(perform: {
            // To remove only extra separators below the list:
            UITableView.appearance().tableFooterView = UIView()

            // To remove all separators including the actual ones:
            UITableView.appearance().separatorStyle = .none
        })
    }
}
```