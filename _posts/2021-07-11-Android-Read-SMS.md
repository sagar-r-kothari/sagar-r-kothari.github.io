---
layout: post
title: "Android - Read SMS"
date: 2021-07-21 11:00:00 +0530
categories: Android SMS Fragments
---

Step 1. Open `AndroidManifest.xml` and add following permission.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.datarobot.spamsmsdetector">
    
    <uses-permission android:name="android.permission.READ_SMS" />
    <application
      ...
```

Step 2. Open your fragment & add following code to check for permissions.

```kotlin
private fun setupView() {
  var array = arrayOf("android.permission.READ_SMS")
  if (checkSelfPermission(requireContext(), android.Manifest.permission.READ_SMS)
      !== PackageManager.PERMISSION_GRANTED) {
      if (ActivityCompat.shouldShowRequestPermissionRationale(requireActivity(),
              android.Manifest.permission.READ_SMS)) {
          requestPermissions(
              arrayOf(android.Manifest.permission.READ_SMS), 1)
      } else {
          requestPermissions(
              arrayOf(android.Manifest.permission.READ_SMS), 1)
      }
  } else {
      // readSms() - your custom logic here
  }
}
```

Step 3. Handle Permissions results.

```kotlin
override fun onRequestPermissionsResult(
    requestCode: Int,
    permissions: Array<out String>,
    grantResults: IntArray
) {
    when (requestCode) {
        1 -> {
            if (grantResults.isNotEmpty() && grantResults[0] ==
                PackageManager.PERMISSION_GRANTED) {
                if ((checkSelfPermission(requireActivity(),
                        android.Manifest.permission.READ_SMS) ===
                            PackageManager.PERMISSION_GRANTED)) {
                    Toast.makeText(requireContext(), "Permission Granted", Toast.LENGTH_SHORT).show()
                    // readSms() - your custom logic here
                }
            } else {
                Toast.makeText(requireContext(), "Permission Denied", Toast.LENGTH_SHORT).show()
            }
            return
        }
    }
}
```

Step 4. Read SMS

```kotlin
private fun readSms() {
    val uri = Uri.parse("content://sms/inbox")
    val cursor = requireActivity()
        .contentResolver.query(
            uri,
            null, null, null, null
        ) ?: return
    val list = mutableListOf<String>()
    if (cursor.moveToFirst()) { // must check the result to prevent exception
        do {
            var msgData = ""
            for (idx in 0 until cursor.columnCount) {
                if (cursor.getColumnName(idx).toString() == "body") {
                    msgData = cursor.getString(idx)
                }
            }
            Log.d("Dashboard", "SMS: $msgData")
            list += msgData
        } while (cursor.moveToNext())
        // bind list to recyclerView
        binding.recyclerView.adapter = DashboardAdapter(list.toTypedArray()) { index ->
            Log.d("Dashboard", "Clicked at index - $index")
        }
        cursor.close()
    } else {
        Log.d("Dashboard", "no SMS")
    }
}
```