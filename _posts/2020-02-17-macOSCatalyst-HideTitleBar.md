---
layout: post
title: "macOS Catalyst - Hide Title Bar"
date: 2020-02-17 08:20:00 +0530
categories: macOSCatalyst
---

1. Open your Project.
2. Open `SceneDelegate.swift`
3. Modify `scene(_ scene, willConnectTo)` method as illustrated below.

```swift
@available(iOS 13.0, *)
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
  var window: UIWindow?

  func scene(
    _ scene: UIScene, 
    willConnectTo session: UISceneSession, 
    options connectionOptions: UIScene.ConnectionOptions
  ) {
    guard let windowScene = (scene as? UIWindowScene) else { return }
    #if targetEnvironment(macCatalyst)
    if let titlebar = windowScene.titlebar {
      titlebar.titleVisibility = .hidden
      titlebar.toolbar = nil
    }
    #endif
  }
}
```