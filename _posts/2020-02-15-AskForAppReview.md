---
layout: post
title: "Swift - Ask for AppStore Review from your app"
date: 2020-02-15 08:20:00 +0530
categories: Swift
---

Replace following with your app URL.

```swift
func requestReviewTapped() {
  let text = "https://apps.apple.com/app/moneymeter/id1482614257?action=write-review"
  if let url = URL(string: text) {
    UIApplication.shared.open(url, options: [: ], completionHandler: nil)
  }
}
```