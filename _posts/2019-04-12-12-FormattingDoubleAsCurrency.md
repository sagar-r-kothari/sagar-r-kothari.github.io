---
layout: post
title:  "Formatting double as currency"
date:   2019-04-12 12:00:00 +0530
categories: Swift
---

```swift
extension Double {
	var formattedCurrencyString: String? {
		let formatter = NumberFormatter()
		formatter.locale = Locale.current
		formatter.numberStyle = .currency
		return formatter.string(from: self as NSNumber)
	}
}
```