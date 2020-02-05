---
layout: post
title: "Binary Tree - Insert"
date: 2020-02-05 08:20:00 +0530
categories: BinaryTree DataStructure
---

```swift
import Foundation

class BinaryTreeNode {
    let value: Int
    var left: BinaryTreeNode?
    var right: BinaryTreeNode?
    init(_ value: Int) {
        self.value = value
    }
}
```

```swift
struct BinaryTree {
    var root: BinaryTreeNode?

    mutating func insert(value: Int) {
        guard
            let node = root
            else {
                root = BinaryTreeNode(value)
                return
        }
        var array = [BinaryTreeNode]()
        array.append(node)
        while let first = array.first {
            array.remove(at: 0)
            if let left = first.left {
                array.append(left)
            } else {
                first.left = BinaryTreeNode(value)
                return
            }
            if let right = first.right {
                array.append(right)
            } else {
                first.right = BinaryTreeNode(value)
                return
            }
        }
    }
}
```