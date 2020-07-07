---
layout: post
title: "Android - Kotlin - Change Action bar title"
date: 2020-07-02 08:20:00 +0530
categories: Android Kotlin
---


### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        // -------------------------
        supportActionBar?.title = "Github Users"
        // -------------------------
    }
}
```