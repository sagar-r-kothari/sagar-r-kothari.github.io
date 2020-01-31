---
layout: post
title: "Singly Link List - Delete Last"
date: 2020-01-31 10:20:00 +0530
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

    mutating func deleteAtEnd() {
        var current = head
        while current?.next?.next != nil {
            current = current?.next
        }
        current?.next = nil
    }
}
```