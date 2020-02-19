---
layout: post
title: "Swift - check or access Size classes via code"
date: 2020-02-18 08:20:00 +0530
categories: Swift
---

Put following lines of code in your viewController. 
E.g. I've added code in `viewDidLoad`

```swift
class YourViewController: UIViewController {
  override func viewDidLoad() {
    super.viewDidLoad()

    // Code to check size class <--------------
    if traitCollection.horizontalSizeClass == .compact {
      // update view as per compact settings
    } else {
      // update your view as per regular settings
    }
    // --- end
  }
}
```