---
layout: post
title: "Android - Regex"
date: 2021-02-12 08:00:00 +0530
categories: Android
---


```kotlin
// Regex
val regex = Regex("\\d{4}-\\d{2}-\\d{2}")
val someString = "2021-02-12 08:00:00 +0530"

val valueMatchingRegex = regex.findAll(someString).first().value // "2021-02-12"
```

This is just an example. Don't use Regex for dates.

```kotlin
val someString = "Sagar Swift Kotlin 12345"
val stringWithoutNumbers = 
    someString.replace(Regex("[0-9]+", RegexOption.IGNORE_CASE), "")
Lod.d("Regex", "$stringWithoutNumbers") // "Sagar Swift Kotlin "
```