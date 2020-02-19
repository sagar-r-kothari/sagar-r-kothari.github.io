---
layout: post
title: "Swift - Date in dd-MMM or dd-MMM-yy format"
date: 2020-02-18 06:20:00 +0530
categories: Swift
---

I came across this siutation.
Show date without year if it's same year else show with year.

1. E.g. If it's 15-Feb of this year, just show as `15-Feb`
2. E.g. If it's 15-Feb-2017 (other than current year), show with year - `15-Feb-17`

I've created a date extension for above requirement, which is as follows.


```swift
public extension Date {
  var justDateValue: String {
    let calendar = Calendar.current
    let day = calendar.component(.day, from: self)
    let month = calendar.component(.month, from: self)
    let year = calendar.component(.year, from: self)
    let currentYear = calendar.component(.year, from: Date())
    guard
      let date = DateComponents(calendar: .current, timeZone: .current, year: year, month: month, day: day).date
      else { return "" }
    let df = DateFormatter()
    if year == currentYear {
      df.dateFormat = "dd-MMM"
    } else {
      df.dateFormat = "dd-MMM-yy"
    }
    return df.string(from: date)
  }
}
```