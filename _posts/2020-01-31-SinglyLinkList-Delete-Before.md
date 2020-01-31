---
layout: post
title: "Singly Link List - Delete Before"
date: 2020-01-31 09:20:00 +0530
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

    mutating func deleteBefore(_ value: Int) {
        guard head != nil else { return }
        var current = head
        if current?.value == value || current?.next?.value == value {
            head = head?.next
        } else {
            var previous = head
            while current?.next != nil, current?.next?.value != value {
                previous = current
                current = current?.next
            }
            if current?.next?.value == value, previous != nil {
                previous?.next = current?.next
            }
        }
    }
}
```