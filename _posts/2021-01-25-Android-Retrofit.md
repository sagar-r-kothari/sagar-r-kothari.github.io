---
layout: post
title: "Android - Kotlin - Retrofit"
date: 2021-01-25 10:00:00 +0530
categories: Android Kotlin Retrofit
---

### Step 1. Open `build.gradle` (module level)

```
dependencies {
    // ....
    // RetroFit for Server Communication
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    implementation 'com.squareup.okhttp3:logging-interceptor:3.8.0'
    // ....
}
```

### Step 2. Open / Create ServiceBuilder file.

```kotlin
import okhttp3.OkHttpClient
import okhttp3.logging.HttpLoggingInterceptor
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory


object ServiceBuilder {
    private val client = OkHttpClient.Builder()

    private val retrofit = Retrofit.Builder()
            .baseUrl(BuildConfig.ENDPOINT)
            .addConverterFactory(GsonConverterFactory.create())

    fun <T> buildService(service: Class<T>): T {
        // log request, response body
        val logging = HttpLoggingInterceptor()
        logging.level = HttpLoggingInterceptor.Level.BODY
        client.addInterceptor(logging)
        retrofit.client(client.build())
        return retrofit.build().create(service)
    }
}
```

### Step 3. Define Service Interface

```kotlin
import retrofit2.Call
import retrofit2.http.Body
import retrofit2.http.Headers
import retrofit2.http.POST

interface LoginAPI {
    @Headers("Content-Type: application/json")
    @POST("v2/sessions")
    fun login(@Body loginData: LoginRequestInfo): Call<UserResponse>
}
```

### Step 4. Open / Create Service file.

```kotlin
class LoginService {
    fun login(
        email: String,
        password: String,
        twoFacAuth: String?,
        onResult: (UserResponse?, ErrorResponse?) -> Unit) {

        val retrofit = ServiceBuilder.buildService(LoginAPI::class.java)
        val values = SomeCrypto.loginParamsForEmail(email, password)
        
        retrofit.login(values).enqueue(object: Callback<UserResponse> {
            override fun onFailure(call: Call<UserResponse>, t: Throwable) {
                onResult(null, null)
            }

            override fun onResponse(call: Call<UserResponse>, response: Response<UserResponse>) {
                response.body()?.let {
                    onResult(it, null)
                }  ?: run {
                    val error = Gson().fromJson(response.errorBody()?.string(), ErrorResponse:: class.java)
                    onResult(null, error)
                }
            }
        })
    }
}
```