---
layout: post
title: "Android - Touch ID - Device Passcode"
date: 2021-02-07 08:00:00 +0530
categories: Android Kotlin
---

#### Module specific *build.bradle*
```
implementation "androidx.biometric:biometric-ktx:1.2.0-alpha02"
```

#### A function to perform Touch ID or Device Auth

```kotlin
private fun performBioAuth() {
    // Here this is a fragment
    BioAuth.performAuth(this) { success, errCode, errString ->
        if (success) {
            Log.d("BioAuth", "Complete. ToDo Next")
            // let's say perform login!
        } else {
            // oops. show error to user
            Log.d("BioAuth", "Failed ${errCode ?: 0}, ${errString ?: ""}")
        }
    }
}
```

#### A function to setup buttons 
- To show Touch ID button or not
- To show Device passcode button or not

```kotlin
private fun setupAutoLoginButtons() {
    val canBioAuth = BioAuth.doWeHaveFingerPrintAuth(requireContext())
    val canDeviceAuth = BioAuth.doWeHaveDeviceAuth(requireContext())
    // Touch ID button visibility
    binding.imageViewBio.isVisible = canBioAuth
    // Device Passcode button visibility
    binding.imageViewDeviceLock.isVisible = !canBioAuth and canDeviceAuth
}
```

#### An object to handle Authentication

```kotlin
import android.content.Context
import android.os.Build
import android.util.Log
import androidx.biometric.BiometricManager
import androidx.biometric.BiometricManager.Authenticators.*
import androidx.biometric.BiometricManager.BIOMETRIC_SUCCESS
import androidx.biometric.BiometricPrompt
import androidx.fragment.app.Fragment

object BioAuth {
    fun doWeHaveFingerPrintAuth(context: Context): Boolean {
        return when {
            Build.VERSION.SDK_INT >= Build.VERSION_CODES.P -> {
                BiometricManager.from(context).canAuthenticate(
                    BIOMETRIC_STRONG
                ) == BIOMETRIC_SUCCESS
            }
            else -> {
                false
            }
        }
    }

    fun doWeHaveDeviceAuth(context: Context): Boolean {
        return when {
            Build.VERSION.SDK_INT >= Build.VERSION_CODES.R -> {
                BiometricManager.from(context).canAuthenticate(DEVICE_CREDENTIAL) == BIOMETRIC_SUCCESS
            }
            else -> {
                false
            }
        }
    }

    fun performAuth(fragment: Fragment, onResult: (Boolean, Int?, String?) -> Unit) {
        when {
            Build.VERSION.SDK_INT >= Build.VERSION_CODES.P -> {
                val prompt = BiometricPrompt(fragment, object : BiometricPrompt.AuthenticationCallback() {
                    override fun onAuthenticationError(errorCode: Int, errString: CharSequence) {
                        super.onAuthenticationError(errorCode, errString)
                        onResult(false, errorCode, errString.toString())
                    }

                    override fun onAuthenticationSucceeded(result: BiometricPrompt.AuthenticationResult) {
                        super.onAuthenticationSucceeded(result)
                        Log.d("BioAuth", "Success")
                        onResult(true, null, null)
                    }

                    override fun onAuthenticationFailed() {
                        super.onAuthenticationFailed()
                        Log.d("BioAuth", "failed")
                        onResult(false, 404, "Auth failed")
                    }
                })
                val promptInfo = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
                    BiometricPrompt.PromptInfo.Builder()
                        .setAllowedAuthenticators(BIOMETRIC_STRONG or DEVICE_CREDENTIAL)
                        .setTitle("Authenticate")
                        .setSubtitle("Log in using Touch ID or Device Passcode")
                        .build()
                } else {
                    BiometricPrompt.PromptInfo.Builder()
                        .setAllowedAuthenticators(BIOMETRIC_STRONG)
                        .setTitle("Authenticate")
                        .setSubtitle("Log in using Touch ID")
                        .setNegativeButtonText("Use account password")
                        .build()
                }
                prompt.authenticate(promptInfo)
            }
            else -> {
                onResult(false, 404, "No Auth available")
            }
        }
    }
}
```