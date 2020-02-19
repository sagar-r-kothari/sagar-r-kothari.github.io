---
layout: post
title: "Swift - Perform a task in background"
date: 2020-02-14 08:20:00 +0530
categories: Swift
---

```swift
func someFunctionhere() {
	// perform something on background thread
  DispatchQueue.global(qos: .background).async {
    // perform background tasks here


    // Now switch back to Main Thread for UI Operations
    DispatchQueue.main.async {
    	// Update UI here
      MBProgressHUD.hide(for: hudView, animated: false)
    }
  }
}
```