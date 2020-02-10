---
layout: post
title: "Data Structure - Recursion"
date: 2020-02-10 17:20:00 +0530
categories: Swift DataStructure
---

```swift
class Solution {
    var index = 0
    func reverseString(_ s: inout [Character]) {
        guard index < s.count / 2 else { return }
        let temp = s[index]
        s[index] = s[s.count-1-index]
        s[s.count-1-index] = temp
        index += 1
        reverseString(&s)
        return
    }
}
var array = [
    Character("S"),
    Character("A"),
    Character("G"),
    Character("A"),
    Character("R"),
    Character("K"),
    Character("R")
]
Solution().reverseString(&array)
print(array)
```