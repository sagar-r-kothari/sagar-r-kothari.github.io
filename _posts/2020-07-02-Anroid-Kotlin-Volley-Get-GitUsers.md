---
layout: post
title: "Android - Kotlin - Volley - basic get request"
date: 2020-07-02 09:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```
dependencies {
    ...
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.google.code.gson:gson:2.8.5'
    ...
}
```

### User.kt


```kotlin
class User(
    val login: String,
    val id: Int,
    val avatar_url: String
)
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        getUsers()
    }

    /// ------------------------------------------
    fun getUsers() {
    	// 1. Create Queue
        val queue = Volley.newRequestQueue(this)
        // 2. Create Request
        val stringRequest = StringRequest(
            Request.Method.GET, // 2.1 by providing method
            "https://api.github.com/users", // 2.2 url
            Response.Listener<String> { // 2.3 success listner
            	// 2.4 Create gson
                val gson = Gson()
                // 2.5 Create Array Type
                val array = object : TypeToken<Array<User>>() {}.type
                // 2.6 JSON Decode
                var users: Array<User> = gson.fromJson(it, array)
                // 2.7 use as per need.
                println("Users are $users")
                
        }, Response.ErrorListener { // 2.4 error listner
                Toast.makeText(this, "Something went wrong. Try again.", Toast.LENGTH_SHORT).show()
        })
        // 3. add request to queue to execute.
        queue.add(stringRequest)
    }
    /// ------------------------------------------
}
```