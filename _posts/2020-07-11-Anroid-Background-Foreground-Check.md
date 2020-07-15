---
layout: post
title: "Android - Kotlin - Check App is in background or foreground"
date: 2020-07-11 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```rb
dependencies {
    ...
    implementation 'androidx.lifecycle:lifecycle-process:2.2.0'
    ...
}
```


### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity(), LifecycleObserver {
    private var isAppInForeground = false
    override fun onCreate() {
        super.onCreate()
        ProcessLifecycleOwner.get().lifecycle.addObserver(this)
    }

    override fun onDestroy() {
        super.onDestroy()
        ProcessLifecycleOwner.get().lifecycle.removeObserver(this)
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun onForegroundStart() {
        isAppInForeground = true
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun onForegroundStop() {
        isAppInForeground = false
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    // I need to check if app is on foreground or background
    fun someRandomFunction() {
        if (isAppInForeground) {
            println("execute foreground code")
        } else {
            println("execute background code")
        }
    }
}
```