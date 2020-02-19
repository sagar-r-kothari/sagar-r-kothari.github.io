---
layout: post
title: "Swift - Perform a task in background"
date: 2020-02-14 08:20:00 +0530
categories: Swift
---

```swift
func requestReviewTapped() {
  let text = "https://apps.apple.com/app/moneymeter/id1482614257?action=write-review"
  if let url = URL(string: text) {
    UIApplication.shared.open(url, options: [: ], completionHandler: nil)
  }
}
```