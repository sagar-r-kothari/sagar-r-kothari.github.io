---
layout: post
title:  "Android - Kotlin - Validate Email"
date:   2021-01-24 08:00:00 +0530
categories: Android Kotlin
---

### How do I Validate email address?

Here is String extension for email validation.

```kt

// StringExtra.kt

import android.text.TextUtils

fun String.isEmailValid(): Boolean {
    return !TextUtils.isEmpty(this) 
        && android.util.Patterns.EMAIL_ADDRESS.matcher(this).matches()
}
```