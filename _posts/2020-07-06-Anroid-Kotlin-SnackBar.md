
---
layout: post
title: "Android - Kotlin - Showing Snackbar"
date: 2020-07-06 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```
dependencies {
    ...
    implementation 'com.google.android.material:material:1.1.0'
    ...
}
```


```kotlin
Snackbar.make(view, "Here's a Snackbar", Snackbar.LENGTH_LONG)
	.setAction("Action", null)
	.show()
```