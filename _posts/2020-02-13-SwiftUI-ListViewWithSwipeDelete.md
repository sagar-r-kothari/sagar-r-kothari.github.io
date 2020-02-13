---
layout: post
title: "SwiftUI - ListView with Swipe Delete"
date: 2020-02-13 07:20:00 +0530
categories: SwiftUI
---

#### Preview

![Preview Image](/assets/SwifUI-Delete-From-List.gif)

#### Code

```swift
import SwiftUI

struct Employee: Identifiable {
    let id = UUID()
    let name: String
    init(_ name: String) {
        self.name = name
    }
}

struct MyCustomList: View {
    @State var items: [Employee]
    var body: some View {
        NavigationView {
            List {
                ForEach(items) { item in
                    Text(item.name)
                }.onDelete { indexSet in
                    // This one takes care of removing an item from cell
                    self.items.remove(atOffsets: indexSet)
                }
            }
        .navigationBarTitle("Some Title Here")
        }
    }
}

struct MyCustomList_Previews: PreviewProvider {
    static var previews: some View {
        MyCustomList(items: ["Sagar", "Sukrit"].map({ Employee($0) }))
    }
}
```