---
layout: post
title: "Android - Root Check"
date: 2020-07-10 07:20:00 +0530
categories: Android Kotlin
---

### Utilities/RootChecker.kt

```kotlin
import java.io.File

public class RootChecker {
    private fun isRooted(): Boolean {
        return findBinary("su")
    }

    private fun findBinary(binaryName: String): Boolean {
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

    public fun check(context: Context) {
        val activity = context as Activity?
        if (RootChecker().isRooted() && activity != null) {
            AlertDialog.Builder(context)
                .setTitle("Found rooted system")
                .setMessage("Please install on an android which isn't rooted")
                .setPositiveButton("Okay.") { _, _ ->
                    finishAffinity(activity)
                }
                .setOnDismissListener {
                    finishAffinity(activity)
                }
                .setIcon(android.R.drawable.ic_dialog_alert)
                .show()
        }
    }
}
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        RootChecker().check(this)
    }
}
```