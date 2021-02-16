---
layout: post
title: "Android - Changing button background color"
date: 2021-02-10 07:00:00 +0530
categories: Android Kotlin
---

Here is how you apply the background color to a button.

```xml
<Button
android:backgroundTint="@android:color/holo_red_light"
/>
```

We would usually require red colored button for destructive acions such as Delete.

```xml
<Button
    android:id="@+id/someIdentifier"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:backgroundTint="@android:color/holo_red_light"
    android:drawableLeft="@android:drawable/ic_menu_delete"
    android:text="Delete" />
```