---
layout: post
title:  "Adding acknowledgements for pods in your app"
date:   2019-04-12 18:00:00 +0530
categories: Pod CocoaPod
---

Your `Podfile` should look like this.

```py
platform :ios, '10.0'

def common_pods
  pod 'your'
  pod 'pods'
  pod 'here'
end

target 'yourAppTarget' do
  use_frameworks!
  common_pods
end

post_install do | installer_representation |
  require 'fileutils'
  FileUtils.cp_r('Pods/Target Support Files/Pods-yourAppTarget/Pods-yourAppTarget-acknowledgements.plist',
                 'yourAppTarget/Settings.bundle/Acknowledgements.plist',
                 :remove_destination => true)
end
```

Your Settings bundle should have an entry for `Acknowledgements`.
Here is an example.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>StringsTable</key>
  <string>Root</string>
  <key>PreferenceSpecifiers</key>
  <array>
    <dict>
      <key>File</key>
      <string>Acknowledgements</string>
      <key>Title</key>
      <string>Acknowledgements</string>
      <key>Type</key>
      <string>PSChildPaneSpecifier</string>
    </dict>
  </array>
</dict>
</plist>
```