---
layout: post
title:  "Android - Kotlin - Dismiss Keyboard"
date:   2021-01-24 07:00:00 +0530
categories: Android Kotlin
---

### How do I dismiss Android Keyboard?

```kotlin
    fun hideKeyboard() {
        val activity = requireActivity()
        val imm = activity.getSystemService(INPUT_METHOD_SERVICE) as InputMethodManager
        var view = activity.currentFocus
        if (view == null) {
            view = View(activity)
        }
        imm.hideSoftInputFromWindow(view.windowToken, 0)
    }
```