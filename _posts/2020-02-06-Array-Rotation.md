---
layout: post
title: "Array Rotation"
date: 2020-02-06 04:20:00 +0530
categories: Array DataStructure
---

```swift
import Foundation

var array = [1,2,3,4,5,6,7,8,9]

extension Array {
    mutating func rotate(count: Int) {
        for _ in 0..<count {
            guard let firstElement = first else { return }
            remove(at: 0)
            append(firstElement)
        }
    }
}

array.rotate(count: 5)

```