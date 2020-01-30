---
layout: post
title: "Data Structure - Singly Link List - Insert at begining"
date: 2020-01-30 06:20:00 +0530
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

    mutating func insertAtHead(_ value: Int) {
        guard
            head != nil
            else { head = SinglyLinkListNode(value); return }
        let node = SinglyLinkListNode(value)
        node.next = head
        head = node
    }
}
```