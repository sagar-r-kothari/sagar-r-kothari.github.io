---
layout: post
title: "Android - Kotlin - RecyclerView Clickable"
date: 2020-07-03 08:20:00 +0530
categories: Android Kotlin
---

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"

    <!-- make sure you add this -->
    android:focusable="true" 
    <!-- and this -->
    android:clickable="true" 
    <!-- and this -->
    android:foreground="?android:attr/selectableItemBackground">


    ...
    ...
</androidx.constraintlayout.widget.ConstraintLayout>
```