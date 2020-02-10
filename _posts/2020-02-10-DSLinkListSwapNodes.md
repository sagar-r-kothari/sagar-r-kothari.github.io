---
layout: post
title: "Data Structure - Link List Swap nodes in pair"
date: 2020-02-10 18:20:00 +0530
categories: Swift DataStructure
---

```swift
public class ListNode {
    public var val: Int
    public var next: ListNode?
    public init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func printList(_ head: ListNode?) {
    guard let head = head else { return }
    print("\(head.val)")
    printList(head.next)
}



class Solution {
    var head: ListNode?
    func swapPairs(_ head: ListNode?) -> ListNode? {
        if self.head == nil {
            self.head = head
        }
        guard 
            let headNode = head, 
            let nextNode = headNode.next 
            else { return self.head }
        let temp = nextNode.val
        nextNode.val = headNode.val
        headNode.val = temp
        swapPairs(nextNode.next)
        return self.head
    }
}

let head = ListNode(1)
head.next = ListNode(2)
head.next?.next = ListNode(3)
head.next?.next?.next = ListNode(4)

Solution().swapPairs(head)

printList(head)
```