---
layout: post
title: "Binary Tree - level order traversal"
date: 2020-02-05 07:20:00 +0530
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

    func levelOrderDisplay(_ node: BinaryTreeNode?) {
        var array = [BinaryTreeNode]()
        if let node = node {
            array.append(node)
        }
        while let node = array.first {
            if let left = node.left {
                array.append(left)
            }
            if let right = node.right {
                array.append(right)
            }
            print("\(node.value)")
            array.remove(at: 0)
        }
    }
}
```