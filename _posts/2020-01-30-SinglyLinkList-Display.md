---
layout: post
title: "Singly Link List - Display"
date: 2020-01-30 06:00:00 +0530
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

    func display() {
        guard
            head != nil
            else { print("Empty list"); return }
        var currentNode = head
        print("[", separator: "", terminator: "")
        while currentNode != nil {
            print("\(currentNode?.value ?? 0)", separator: "", terminator: "")
            if currentNode?.next != nil {
                print(", ", separator: "", terminator: "")
            }
            currentNode = currentNode?.next
        }
        print("]", separator: "", terminator: "\n")
    }
}
```