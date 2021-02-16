---
layout: post
title: "Android - Copy text to clipboard"
date: 2021-02-11 08:00:00 +0530
categories: Android
---

```kotlin
val clipMan = requireActivity()
        .getSystemService(Context.CLIPBOARD_SERVICE) as ClipboardManager
val clip = ClipData.newPlainText("simple text", "TEXT TO COPY")
clipMan.setPrimaryClip(clip)
```