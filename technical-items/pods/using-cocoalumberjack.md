---
description: Swift Code snippet for using CocoaLumberJack
---

# Using CocoaLumberJack

```swift
import CocoaLumberjack

class AppLogger {

  let fileLogger: DDFileLogger = DDFileLogger() // File Logger

  init() {
    DDLog.add(DDTTYLogger.sharedInstance) // TTY = Xcode console
    DDTTYLogger.sharedInstance.colorsEnabled = true
    fileLogger.rollingFrequency = TimeInterval(60*60*24)  // 24 hours
    fileLogger.logFileManager.maximumNumberOfLogFiles = 7
    DDLog.add(fileLogger)
  }

  public class var shared: AppLogger {
    get {
      struct Single {
        static var shared = AppLogger()
      }
      return Single.shared
    }
  }

}

func logVerbose(_ message: String) {
  DDLogVerbose(message)
}

func logDebug(_ message: String) {
  DDLogDebug(message)
}

func logInfo(_ message: String) {
  DDLogInfo(message)
}

func logWarning(_ message: String) {
  DDLogWarn(message)
}

func logError(_ message: String) {
  DDLogError(message)
}
```

