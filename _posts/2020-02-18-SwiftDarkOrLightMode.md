---
layout: post
title: "Swift - check - Dark mode or light mode"
date: 2020-02-18 07:20:00 +0530
categories: Swift
---

Put following lines of code in your viewController. 
E.g. I've added code in `viewDidLoad`

```swift
class YourViewController: UIViewController {
  override func viewDidLoad() {
    super.viewDidLoad()

    // Code to check dark/light mode <--------------
    if #available(iOS 12.0, *) {
      switch UIScreen.main.traitCollection.userInterfaceStyle {
      case .dark:
        // example settings
        calendar.appearance.subtitleDefaultColor = .lightGray
        calendar.appearance.titleDefaultColor = .white
        calendar.backgroundColor = .systemBackground
      default:
        // example settings
        calendar.appearance.subtitleDefaultColor = .black
        calendar.appearance.titleDefaultColor = .lightGray
        calendar.backgroundColor = .white
      }
    } else {
      // fallback to default 
      // your view's default settings here
    }
    // --- end
  }
}
```