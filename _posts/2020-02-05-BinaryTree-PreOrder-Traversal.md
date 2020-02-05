---
layout: post
title: "Binary Tree - Preorder Traversal"
date: 2020-02-05 04:20:00 +0530
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

    func preOrderDisplay(_ node: BinaryTreeNode?) {
        guard let node = node else { return }
        print("\(node.value) ")
        preOrderDisplay(node.left)
        preOrderDisplay(node.right)
    }
}
```