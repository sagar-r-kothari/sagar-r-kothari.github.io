---
description: Check if your app is running live / app from appStore?
---

# isRunningLive

```swift
public static var isRunningLive: Bool {
  #if TARGET_OS_SIMULATOR
    return false
  #else
    let isRunningTestFlightBeta  = (Bundle.main.appStoreReceiptURL?.lastPathComponent == "sandboxReceipt")
    let hasEmbeddedMobileProvision = !(Bundle.main.path(forResource: "embedded", ofType: "mobileprovision") != nil)
    if isRunningTestFlightBeta || hasEmbeddedMobileProvision {
      return false
    } else {
      return true
    }
  #endif
}
```

