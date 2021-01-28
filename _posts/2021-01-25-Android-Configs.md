---
layout: post
title: "Android - Debug and Release config"
date: 2021-01-25 09:00:00 +0530
categories: Android
---

### Step 1. Open `build.gradle` (module level)

```
android {
    compileSdkVersion 30
    buildToolsVersion "30.0.3"

    defaultConfig {
        applicationId "com.sagar.test"
        minSdkVersion 16
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        debug {
            buildConfigField('String', 'ENDPOINT', '"https://debug.sagar-r-kothari.com/api/"')
        }
        release {
            buildConfigField('String', 'ENDPOINT', '"https://sagar-r-kothari.com/api/"')
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    buildFeatures.dataBinding = true
}
```

### Step 2: Sync project & open `some-xyz.kt` file

Open file where you want to access above defined config.

```kotlin
object ServiceBuilder {
    private val client = OkHttpClient.Builder()

    private val retrofit = Retrofit.Builder()
            .baseUrl(BuildConfig.ENDPOINT)
            .addConverterFactory(GsonConverterFactory.create())
}
```
