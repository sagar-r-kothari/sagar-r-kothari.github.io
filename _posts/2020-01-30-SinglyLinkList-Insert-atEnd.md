---
layout: post
title: "Data Structure - Singly Link List - Insert at End"
date: 2020-01-30 07:20:00 +0530
categories: Swift Singly Link List Data Structure
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

    mutating func insertAtEnd(_ value: Int) {
        guard
            head != nil, let tail = tail
            else { head = SinglyLinkListNode(value); return }
        let node = SinglyLinkListNode(value)
        tail.next = node
    }
}
```