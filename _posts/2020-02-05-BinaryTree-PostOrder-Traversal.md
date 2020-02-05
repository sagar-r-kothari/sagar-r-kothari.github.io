---
layout: post
title: "Binary Tree - post order traversal"
date: 2020-02-05 05:20:00 +0530
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

    func postOrderDisplay(_ node: BinaryTreeNode?) {
        guard let node = node else { return }
        postOrderDisplay(node.left)
        postOrderDisplay(node.right)
        print("\(node.value) ")
    }
}
```