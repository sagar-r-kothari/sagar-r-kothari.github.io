---
layout: post
title: "Binary Tree - Search"
date: 2020-02-05 09:20:00 +0530
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

    func searchElement(_ node: BinaryTreeNode?, value: Int) {
        guard let node = node else { return }
        var array = [BinaryTreeNode]()
        array.append(node)
        while let first = array.first {
            if first.value == value {
                print("Element found")
                return
            }
            array.remove(at: 0)
            if let left = first.left {
                array.append(left)
            }
            if let right = first.right {
                array.append(right)
            }
        }
    }
}
```