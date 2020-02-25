---
layout: post
title: "Swift - Resize UIImage - Code Snippet"
date: 2020-02-25 07:20:00 +0530
categories: Swift
---

```swift
import UIKit

extension UIImage {
   func resizedImage(newWidth: CGFloat) -> UIImage {
       guard size.width > newWidth else { return self }
       let scale = newWidth / self.size.width
       let newHeight = self.size.height * scale
       let newSize = CGSize(width: newWidth, height: newHeight)
       let renderer = UIGraphicsImageRenderer(size: newSize)
       let image = renderer.image { (context) in
           self.draw(in: CGRect(origin: CGPoint(x: 0, y: 0), size: newSize))
       }
       return image
   }
}
```