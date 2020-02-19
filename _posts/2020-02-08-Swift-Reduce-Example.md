---
layout: post
title: "Swift Functional Programming - Reduce"
date: 2020-02-08 08:20:00 +0530
categories: Swift
---

```swift
let items = [10, 20, 5324, 23, 2309]

// readable way
let total = items.reduce(0) { (result, nextValue) -> Int in
    return result + nextValue
}

// encrypted way :P
let anotherTotal = items.reduce(0, { $0 + $1 })

print("Total is \(total)")
```