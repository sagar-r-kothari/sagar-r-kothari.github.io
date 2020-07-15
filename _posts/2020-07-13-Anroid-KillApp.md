---
layout: post
title: "Android - Kill App programmatically"
date: 2020-07-13 07:20:00 +0530
categories: Android Kotlin
---

In some cases, developer might want to close app forcefully.

- e.g. App must update to latest version
- e.g. App can not run on rooted device
- e.g. App doesn't meet the hardware requirement - Gyroscope required.

```
finishAffinity()
```

Here is one of the example. Here we're putting a check - if system is rooted, kill the app after showing a dialog.

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
                .setPositiveButton("Okay.") { _, _ ->
                    finishAffinity()
                }
                .setOnDismissListener {
                    finishAffinity()
                }
                .setIcon(android.R.drawable.ic_dialog_alert)
                .show()
        }
    }

}
```