---
layout: post
title: "Avoid installing Pods for macOS Catalyst"
date: 2020-02-19 08:20:00 +0530
categories: macOSCatalyst
---

Put following lines of code at the end of `Podfile`

```git
post_install do |installer|
  installer.pods_project.targets.each do |target|
    if target.name == "Pods-MoneyMeter"
      puts "Updating #{target.name} to exclude AppCenter"
      target.build_configurations.each do |config|
        xcconfig_path = config.base_configuration_reference.real_path
        xcconfig = File.read(xcconfig_path)
        xcconfig.sub!('-framework "AppCenterAnalytics"', '')
        xcconfig.sub!('-framework "AppCenter"', '')
        xcconfig.sub!('-framework "AppCenterCrashes"', '')
        new_xcconfig = xcconfig + 'OTHER_LDFLAGS[sdk=iphone*] = -framework "AppCenterAnalytics" -framework "AppCenter" -framework "AppCenterCrashes" -framework "FSCalendar"'
        File.open(xcconfig_path, "w") { |file| file << new_xcconfig }
      end
    end
  end
end
```