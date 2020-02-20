---
layout: post
title: "Swift - Check iCloud Availibility"
date: 2020-01-29 05:00:00 +0530
categories: iCloud
---

```swift
import CloudKit
struct CloudKitChecker {
  func iCloudAvailable(
    _ handler: @escaping(Result<Void, Error>) -> Void
    ) {
    CKContainer.default().accountStatus { (accountStatus, icloudError) in
      switch accountStatus {
        case .available:
          handler(.success(()))
        default:
          handler(.failure(CKAvailabilityError.iCloudNotAvailable))
      }
    }
  }
}

public enum CKAvailabilityError: Error {
    case iCloudNotAvailable
}
```