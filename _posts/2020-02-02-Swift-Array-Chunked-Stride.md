---
layout: post
title: "Swift - stride - Array chunks into equal pieces"
date: 2020-02-02 07:20:00 +0530
categories: Swift
---

```swift
public extension Array {
	func chunked(into size: Int) -> [[Element]] {
		return stride(from: 0, to: count, by: size).map {
			Array(self[$0 ..< Swift.min($0 + size, count)])
		}
	}
}
let records = [1, 2, 3, 4, 5, 6, 7, 8, 9]
let chunks = records.chunked(into: 2)
// chunks = [[1, 2], [3, 4], [5, 6], [7, 8], [9]]
```