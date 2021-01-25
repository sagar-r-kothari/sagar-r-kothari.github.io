---
layout: post
title:  "Android - Kotlin - Fragment Layout Binding"
date:   2021-01-24 09:00:00 +0530
categories: Android Kotlin Layout
---

### Project Level Dependency

```gradle
buildscript {
    ext.kotlin_version = "1.4.21"
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.2.0-beta03'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.3.2" // <-- this
    }
}
```

### Module Level Dependency

```gradle
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'androidx.navigation.safeargs.kotlin'
}
```

### Your fragment should be wrapped inside `<layout>` tag

example:
```
<layout
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <!-- 
    Your layout code
    -->
</layout>
```

### Open `SomeFragment.kt` file.

```kt
class SomeFragment : Fragment() {
    private var _binding: FragmentLoginBinding? = null
    private val binding get() = _binding!!
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentLoginBinding.inflate(inflater, container, false)

        // your logic here.
        // example sign-up button click listener
        binding.signUpButton.setOnClickListener {
            val action = LoginFragmentDirections.actionLoginFragmentToSignUpFragment()
            requireView().findNavController().navigate(action)
        }

        return binding.root
    }
}
```