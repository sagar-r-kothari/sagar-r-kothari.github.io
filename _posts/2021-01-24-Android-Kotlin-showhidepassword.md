---
layout: post
title:  "Android - Kotlin - Show hide password on input field"
date:   2021-01-24 10:00:00 +0530
categories: Android Kotlin
---

### How do I show or hide password?

```
binding.showPasswordImageView.setOnClickListener {
    val type = if (binding.textInputEditPassword.inputType != 144) {
        144 // Visible Password
    } else {
        129 // Invisible Password
    }
    binding.textInputEditPassword.inputType = type
}
```