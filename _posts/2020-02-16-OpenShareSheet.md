---
layout: post
title: "Swift - Open Share sheet"
date: 2020-02-16 08:20:00 +0530
categories: Swift
---

```swift
func openShareSheet(from button: UIButton) {
  let appURLText = "https://apps.apple.com/app/moneymeter/id1482614257"
  let viewController = UIActivityViewController(
    activityItems: [appURLText], applicationActivities: nil
  )
  viewController.modalPresentationStyle = .popover
  viewController.popoverPresentationController?.sourceView = button.infoButton
  viewController.popoverPresentationController?.sourceRect = button.infoButton.frame
  present(viewController, animated: true, completion: nil)
}
```