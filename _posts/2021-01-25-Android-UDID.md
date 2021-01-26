---
layout: post
title: "Android - Get UDID Unique device identifier"
date: 2021-01-25 08:00:00 +0530
categories: Android
---

```kt
import android.provider.Settings

val android_id = Settings.Secure.getString(context.contentResolver, Settings.Secure.ANDROID_ID);
```