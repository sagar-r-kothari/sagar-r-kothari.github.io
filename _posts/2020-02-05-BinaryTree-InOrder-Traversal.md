---
layout: post
title: "Binary Tree - in order traversal"
date: 2020-02-05 06:20:00 +0530
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

    func inOrderDisplay(_ node: BinaryTreeNode?) {
        guard let node = node else { return }
        inOrderDisplay(node.left)
        print("\(node.value) ")
        inOrderDisplay(node.right)
    }
}
```