---
layout: post
title: "Android - Secure Shared Preferences"
date: 2021-01-26 07:00:00 +0530
categories: Android Kotlin Secure
---

```kotlin
import android.content.Context
import android.os.Build
import android.security.keystore.KeyGenParameterSpec
import android.security.keystore.KeyProperties
import androidx.annotation.RequiresApi
import androidx.security.crypto.EncryptedSharedPreferences
import androidx.security.crypto.MasterKey

@RequiresApi(Build.VERSION_CODES.M)
object MyAppSecurePreferences {
    private const val preferencesName = "com-my-app-pref" // change this
    private const val tokenPreferenceIdentifier = "tokenPreferenceIdentifier"
    private val spec = KeyGenParameterSpec.Builder(
            MasterKey.DEFAULT_MASTER_KEY_ALIAS,
            KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT)
            .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
            .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
            .setKeySize(MasterKey.DEFAULT_AES_GCM_MASTER_KEY_SIZE)
            .build()

    @Synchronized private fun set(key: String, value: String, context: Context) {
        val masterKey = MasterKey.Builder(context)
                .setKeyGenParameterSpec(spec)
                .build()
        val sharedPreferences = EncryptedSharedPreferences.create(
                context,
                preferencesName,
                masterKey,
                EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
                EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM)
        val editor = sharedPreferences.edit()
        editor.putString(key, value)
        editor.commit()
    }

    private fun get(key: String, context: Context): String? {
        val masterKey = MasterKey.Builder(context)
                .setKeyGenParameterSpec(spec)
                .build()
        val sharedPreferences = EncryptedSharedPreferences.create(
                context,
                preferencesName,
                masterKey,
                EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
                EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM);
        return sharedPreferences.getString(key, "")
    }

    @Synchronized fun setToken(token: String, context: Context) {
        set(tokenPreferenceIdentifier, token, context)
    }

    fun getToken(context: Context): String? {
        return get(tokenPreferenceIdentifier, context)
    }
}
```