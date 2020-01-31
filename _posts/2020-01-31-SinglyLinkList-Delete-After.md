---
layout: post
title: "Singly Link List - Delete After"
date: 2020-01-31 08:20:00 +0530
categories: SinglyLinkList DataStructure
---

```swift
import Foundation

class SinglyLinkListNode {
    let value: Int
    var next: SinglyLinkListNode?
    init(_ value: Int) {
        self.value = value
        next = nil
    }
}
```

```swift
struct SinglyLinkList {
    var head: SinglyLinkListNode?
    var tail: SinglyLinkListNode? {
        guard
            head != nil
            else { return nil }
        var currentNode = head
        while currentNode?.next != nil {
            currentNode = currentNode?.next
        }
        return currentNode
    }

    mutating func deleteAfter(_ value: Int) {
        var current = head
        while current != nil, current?.value != value {
            current = current?.next
        }
        if current != nil {
            current?.next = current?.next?.next
            current?.next?.next = nil
        }
    }
}
```