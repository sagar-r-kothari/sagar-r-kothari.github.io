---
description: Swift 5.1 Property Wrappers by Example
---

# Property Wrappers

#### Example 1

```swift
import Foundation

@propertyWrapper
struct Trimmed {
    private(set) var value: String = ""

    var wrappedValue: String {
        get { value }
        set { value = newValue.trimmingCharacters(in: .whitespacesAndNewlines) }
    }

    init(initialValue: String) {
        self.wrappedValue = initialValue
    }
}
```

Here, upon setting value, it makes sure that newValue is trimmed before getting set. You might be wondering, we can do same with didSet. But I guess, @propertyWrapper is better solution because didSet won't be invoked during initialization.

Here is the usage example

```swift
struct Article {
    @Trimmed var title: String
    @Trimmed var body: String
}

let testArticle = Article(title: "  Swift Property Wrappers  ", body: "...")
testArticle.title // "Swift Property Wrappers" (no leading or trailing spaces!)

testArticle.title = "      @propertyWrapper     "
testArticle.title // "@propertyWrapper" (still no leading or trailing spaces!)
```

