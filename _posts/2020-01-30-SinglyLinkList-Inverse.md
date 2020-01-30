---
layout: post
title: "Data Structure - Singly Link List - Inverse"
date: 2020-01-30 06:10:00 +0530
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

    mutating func inverse() {
        guard head != nil else { return }
        var current = head
        var prev, next: SinglyLinkListNode?
        while current != nil {
            next = current?.next
            current?.next = prev
            prev = current
            current = next
        }
        head = prev
    }
}
```