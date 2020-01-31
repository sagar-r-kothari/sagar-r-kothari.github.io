---
layout: post
title: "Data Structure - Singly Link List - Insert Before"
date: 2020-01-31 07:20:00 +0530
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

    mutating func insertBefore(_ value: Int, searchValue: Int) {
        guard
            head != nil
            else { head = SinglyLinkListNode(value); return }
        var currentNode = head
        let newNode = SinglyLinkListNode(value)
        if currentNode?.value == searchValue {
            newNode.next = currentNode
            head = newNode
        } else {
            while currentNode?.next != nil, currentNode?.next?.value != searchValue {
                currentNode = currentNode?.next
            }
            if currentNode?.next == nil {
                currentNode?.next = newNode
            } else {
                newNode.next = currentNode?.next
                currentNode?.next = newNode
            }
        }
    }
}
```