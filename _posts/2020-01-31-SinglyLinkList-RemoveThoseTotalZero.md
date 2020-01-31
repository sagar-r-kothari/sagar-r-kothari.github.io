---
layout: post
title: "Singly Link List - Remove those total is zero"
date: 2020-01-31 12:20:00 +0530
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

    
    mutating func removeWhoseTotalIsZero() {
        var current = self.head
        while current != nil {
            var fast = current?.next
            var nodeDeleted = false
            while fast != nil {
                if let fastValue = fast?.value, let currentValue = current?.value,
                    (fastValue + currentValue) == 0 {
                    nodeDeleted = true
                    current = current?.next
                    fast = fast?.next
                    delete(fastValue)
                    delete(currentValue)
                } else {
                    fast = fast?.next
                }
            }
            if !nodeDeleted { current = current?.next }
        }
    }
}
```