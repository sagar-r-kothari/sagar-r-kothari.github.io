---
layout: post
title:  "Swift Date Extension"
date:   2019-04-12 15:00:00 +0530
categories: Swift Date
---

```swift
extension Date {
  var startOfWeek: Date? {
    let gregorian = Calendar(identifier: .gregorian)
    guard let sunday = gregorian.date(from: gregorian.dateComponents([.yearForWeekOfYear, .weekOfYear], from: self)) else { return nil }
    return gregorian.date(byAdding: .day, value: 1, to: sunday)
  }
  var endOfWeek: Date? {
    let gregorian = Calendar(identifier: .gregorian)
    guard let sunday = gregorian.date(from: gregorian.dateComponents([.yearForWeekOfYear, .weekOfYear], from: self)) else { return nil }
    return gregorian.date(byAdding: .day, value: 7, to: sunday)
  }
  var startOfLastWeek: Date? {
    let gregorian = Calendar(identifier: .gregorian)
    guard let sunday = gregorian.date(from: gregorian.dateComponents([.yearForWeekOfYear, .weekOfYear], from: self)) else { return nil }
    return gregorian.date(byAdding: .day, value: -6, to: sunday)
  }
  var endOfLastWeek: Date? {
    let gregorian = Calendar(identifier: .gregorian)
    guard let sunday = gregorian.date(from: gregorian.dateComponents([.yearForWeekOfYear, .weekOfYear], from: self)) else { return nil }
    return gregorian.date(byAdding: .day, value: 0, to: sunday)
  }
  var startOfMonth: Date? {
    return Calendar.current.date(from: Calendar.current.dateComponents([.year, .month], from: Calendar.current.startOfDay(for: self)))
  }
  var endOfMonth: Date? {
    guard let startOfMonthHere = startOfMonth else { return nil }
    return Calendar.current.date(byAdding: DateComponents(month: 1, day: -1), to: startOfMonthHere)
  }
  var startOfLastMonth: Date? {
    guard let startOfMonthHere = startOfMonth else { return nil }
    return Calendar.current.date(byAdding: DateComponents(month: -1), to: startOfMonthHere)
  }
  var endOfLastMonth: Date? {
    guard let startOfMonthHere = startOfMonth else { return nil }
    return Calendar.current.date(byAdding: DateComponents(day: -1), to: startOfMonthHere)
  }
}
```