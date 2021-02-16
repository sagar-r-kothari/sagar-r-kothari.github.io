---
layout: post
title: "Android - Set button icon"
date: 2021-02-10 08:00:00 +0530
categories: Android Layout
---

Here is how you apply the an icon to left to a button.

```xml
<Button
android:drawableLeft="@android:drawable/ic_menu_delete"
/>
```

```xml
<Button
    android:id="@+id/someIdentifier"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:backgroundTint="@android:color/holo_red_light"
    android:drawableLeft="@android:drawable/ic_menu_delete"
    android:text="Delete" />
```