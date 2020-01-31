---
layout: post
title: "Singly Link List - Remove Loop"
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

    mutating func createLoop() {
        head = SinglyLinkListNode(10)
        head?.next = SinglyLinkListNode(20)
        head?.next?.next = SinglyLinkListNode(30)
        head?.next?.next?.next = SinglyLinkListNode(40)
        head?.next?.next?.next?.next = SinglyLinkListNode(50)
        head?.next?.next?.next?.next?.next = head
    }

    func findAndRemoveLoop() {
        var current = self.head
        while current != nil {
            var fast = current?.next
            while fast != nil {
                if fast?.next?.value == current?.value {
                    fast?.next = nil
                }
                fast = fast?.next
            }
            current = current?.next
        }
    }
}
```