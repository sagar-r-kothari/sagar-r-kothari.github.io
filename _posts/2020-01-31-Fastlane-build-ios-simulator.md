---
layout: post
title: "Fastlane just compile or build for iOS Simulator"
date: 2020-01-31 06:15:00 +0530
categories: Swift Fastlane CI-CD
---

1. Add Following lane to your `fastlane` file.

```
  lane :buildApp do
    xcbuild({
      workspace: "MYPROJECT.xcworkspace",
      scheme: "MYTARGET",
      sdk: "iphonesimulator",
      destination: "platform=iOS Simulator,name=iPhone 8",
      xcargs: "ONLY_ACTIVE_ARCH=NO"
    })
  end
```

2. Replace `MYPROJECT` with name of the project.
3. Replace `MYTARGET` with name of the Target.
4. Run command `bundle exec fastlane ios buildApp`
5. Add this to any CI-CD system like Semaphore, Github Actions.
