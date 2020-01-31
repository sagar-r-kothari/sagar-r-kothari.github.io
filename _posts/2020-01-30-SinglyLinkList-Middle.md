---
layout: post
title: "Singly Link List - Display Middle"
date: 2020-01-30 06:15:00 +0530
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

    func displayMiddle() {
        guard head != nil else { return }
        var slow = head
        var fast = head
        while fast != nil {
            slow = slow?.next
            fast = fast?.next?.next
        }
        if let slow = slow {
            print("Link list middle value is \(slow.value)")
        }
    }
}
```