---
layout: post
title: "Swift - Font for Numbers and currency"
date: 2020-02-03 07:20:00 +0530
categories: Swift
---

```swift
class YourViewHere: UIView {
  @IBOutlet var amountLabel: UILabel? {
    didSet {
      guard let label = amountLabel else { return }
      // apply font for numbers
      amountLabel?.font = .monospacedDigitSystemFont(ofSize: label.font.pointSize, weight: .regular)
    }
  }
}
```