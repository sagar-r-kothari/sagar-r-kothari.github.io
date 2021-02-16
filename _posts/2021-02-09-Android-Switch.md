---
layout: post
title: "Android - Using Switch UI Component"
date: 2021-02-09 07:00:00 +0530
categories: Android Kotlin
---

Here is how you add a switch to your layout.

```xml
<androidx.appcompat.widget.SwitchCompat
    android:id="@+id/someSwitchIdentifier"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:checked="false"
    android:text="@string/some_string_value" />
```

Here is how you set the switch state using code.

```kotlin
binding.someSwitchIdentifier.isChecked = true // or false
```

Here is how you observe state change of a switch.

```kotlin
binding.someSwitchIdentifier.setOnCheckedChangeListener{ switch, isChecked ->
    Log.d("Switch", "Do something here")
}
```