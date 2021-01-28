---
layout: post
title: "Android - Get UDID Unique device identifier"
date: 2021-01-25 08:00:00 +0530
categories: Android
---

```kotlin
import android.provider.Settings

val android_id = Settings.Secure.getString(
    context.contentResolver, 
    Settings.Secure.ANDROID_ID
);
```

OR

```kotlin
import android.content.Context
import android.content.SharedPreferences
import java.util.*

object MyAppUUID {
    private var uniqueID: String? = null
    private const val identifierForUuidInPref = "identifierForUuidInPref"

    @Synchronized fun id(context: Context): String? {
        if (uniqueID == null) {
            val sharedPrefs = context.getSharedPreferences(
                    identifierForUuidInPref,
                    Context.MODE_PRIVATE
            )
            uniqueID = sharedPrefs.getString(identifierForUuidInPref, null)
            if (uniqueID == null) {
                uniqueID = UUID.randomUUID().toString()
                val editor: SharedPreferences.Editor = sharedPrefs.edit()
                editor.putString(identifierForUuidInPref, uniqueID)
                editor.commit()
            }
        }
        return uniqueID
    }
}
```