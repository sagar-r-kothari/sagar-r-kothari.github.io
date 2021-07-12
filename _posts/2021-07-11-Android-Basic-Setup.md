---
layout: post
title: "Android - Basic setup"
date: 2021-07-21 10:00:00 +0530
categories: Android NavigationUI Fragments
---

Step 1. Class path in project level `build.gradle`

```
classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.3.5"
```

Step 2. Add following dependencies to `build.gradle` - module level

```
// Navigation Fragments
implementation 'androidx.navigation:navigation-fragment-ktx:2.3.5'
implementation 'androidx.fragment:fragment-ktx:1.3.5'

// Navigation UI
implementation 'androidx.navigation:navigation-ui-ktx:2.3.5'
```

Step 3. Add following plugins to `build.gradle` - module level

```
id 'androidx.navigation.safeargs.kotlin'
id 'kotlin-kapt'
```

Step 4. Enable Data Binding in `build.gradle`

```
buildFeatures {
  dataBinding true
  viewBinding true
}
```

Step 5. Add Necessary fragments and add those in navigation.xml (under res/navigation/navigation.xml)

Step 6. Configure `activity_main.xml` as follows.

```xml
<fragment
  android:id="@+id/myNavHostFragment"
  android:name="androidx.navigation.fragment.NavHostFragment"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  app:defaultNavHost="true"
  app:navGraph="@navigation/navigation" />
```

Step 7. Change MainActivity code as follows (inside onCreate)

```kotlin
val navController = this.findNavController(R.id.myNavHostFragment)
NavigationUI.setupActionBarWithNavController(this, navController)
```

Step 8. To set up, navigation up, add following code.

```kotlin
Step 7.
override fun onSupportNavigateUp(): Boolean {
    val navController = this.findNavController(R.id.myNavHostFragment)
    return navController.navigateUp()
}
```