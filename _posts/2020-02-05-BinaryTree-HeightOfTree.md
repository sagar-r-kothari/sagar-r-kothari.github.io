---
layout: post
title: "Binary Tree - Find height of a tree"
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

    func height(_ node: BinaryTreeNode?) -> Int {
        guard let node = node else { return 0 }
        let leftHeight = height(node.left) + 1
        let rightHeight = height(node.right) + 1
        return leftHeight > rightHeight ? leftHeight : rightHeight
    }
}
```