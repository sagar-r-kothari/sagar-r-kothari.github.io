---
layout: post
title:  "Changing UIBarButtonItem Font"
date:   2019-04-13 10:00:00 +0530
categories: UIKit
---

```swift
let barButton = UIBarButtonItem(title: "Text", style: .plain, target: nil, action: nil)
barButton.setTitleTextAttributes(
  [.font : UIFont.systemFont(ofSize: 20)],
  for: .normal)
barButton.setTitleTextAttributes(
  [.font : UIFont.systemFont(ofSize: 20)],
  for: .highlighted)
```