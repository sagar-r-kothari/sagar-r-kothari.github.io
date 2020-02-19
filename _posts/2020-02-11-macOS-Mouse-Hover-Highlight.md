---
layout: post
title: "macOS Catalyst - Mouse Hover - hightlight"
date: 2020-02-11 08:20:00 +0530
categories: macOSCatalyst
---

```swift
import UIKit

class YourTableViewCell: UITableViewCell { // 1. It can be any view
  var color: UIColor? // 2. make sure you save color

  required init?(coder: NSCoder) {
    super.init(coder: coder)
    DispatchQueue.main.asyncAfter(deadline: .now() + 0.2) {
      if #available(iOS 13.0, *) {
      	// 3. Add hover gesture and save actual color applied in storyboard / xib
        let hover = UIHoverGestureRecognizer(target: self, action: #selector(self.hovering(_:)))
        self.addGestureRecognizer(hover)
        self.color = self.backgroundColor
      }
    }
  }

  // Switch colors upon hover
  @available(iOS 13.0, *)
  @objc func hovering(_ recognizer: UIHoverGestureRecognizer) {
    switch recognizer.state {
      case .began, .changed:
        backgroundColor = .lightGray
      case .ended:
        backgroundColor = color
      default:
        backgroundColor = color
    }
  }
}
```