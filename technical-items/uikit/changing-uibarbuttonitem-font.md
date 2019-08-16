---
description: Code snippet for changing UIBarButtonItem Font
---

# Changing UIBarButtonItem Font

```swift
let barButton = UIBarButtonItem(
  title: "Text", 
  style: .plain, 
  target: nil, 
  action: nil
)

barButton.setTitleTextAttributes(
  [.font : UIFont.systemFont(ofSize: 20)],
  for: .normal
)

barButton.setTitleTextAttributes(
  [.font : UIFont.systemFont(ofSize: 20)],
  for: .highlighted
)
```

