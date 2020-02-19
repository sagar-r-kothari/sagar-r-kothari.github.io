---
layout: post
title: "Swift Equatable - supporting == operator on your data structure"
date: 2020-02-09 08:20:00 +0530
categories: Swift
---

```swift
struct MyDataStructure: Equatable {
  let name: String
  let employeeId: String

  static func == (lhs: MyDataStructure, rhs: MyDataStructure) -> Bool {
    return lhs.employeeId == rhs.employeeId
  }
}
```