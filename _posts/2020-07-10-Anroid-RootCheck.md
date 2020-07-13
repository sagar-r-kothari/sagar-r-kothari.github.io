---
layout: post
title: "Android - Root Check"
date: 2020-07-10 07:20:00 +0530
categories: Android Kotlin
---

### Utilities/RootChecker.kt

```kotlin
import java.io.File

class RootChecker {
    fun isRooted(): Boolean {
        return findBinary("su")
    }

    fun findBinary(binaryName: String): Boolean {
        var found = false
        if (!found) {
            val places = arrayOf(
                "/sbin/", "/system/bin/", "/system/xbin/", "/data/local/xbin/",
                "/data/local/bin/", "/system/sd/xbin/", "/system/bin/failsafe/", "/data/local/"
            )
            for (where in places) {
                if (File(where + binaryName).exists()) {
                    found = true
                    break
                }
            }
        }
        return found
    }
}
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        if (RootChecker().isRooted()) {
            AlertDialog.Builder(this)
                .setTitle("Found rooted system")
                .setMessage("Please install on an android which isn't rooted")
                .setPositiveButton("Ohh! Okay.") { _, _ ->
                    finishAffinity()
                }
                .setOnDismissListener {
                    println("Oh dismissed.")
                    finishAffinity()
                }
                .setIcon(android.R.drawable.ic_dialog_alert)
                .show()
        }
    }

}
```