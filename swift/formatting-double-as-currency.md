---
description: 'Swift, Double, Extension - to format Double as currency'
---

# Formatting Double as currency

```swift
extension Double {
    var formattedCurrencyString: String? {
        let formatter = NumberFormatter()
        formatter.locale = Locale.current
        formatter.numberStyle = .currency
        return formatter.string(from: self as NSNumber)
    }
}
```

