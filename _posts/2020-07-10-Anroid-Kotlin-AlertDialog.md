---
layout: post
title: "Android - Kotlin - Show AlertDialog"
date: 2020-07-10 08:20:00 +0530
categories: Android Kotlin
---

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        AlertDialog.Builder(this)
            .setTitle("App Update Available")
            .setMessage("Would you like to update App?")
            .setPositiveButton("Yes") { _, _ ->
                println("Do something to update app.")
            }
            .setNegativeButton("No", null)
            .setOnDismissListener {
                println("Oh! User dismissed without pressing any button.")
                finishAffinity()
            }
            .setIcon(android.R.drawable.ic_dialog_alert)
            .show()
    }

}
```