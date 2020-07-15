---
layout: post
title: "Android - Kotlin - EventBus - Communication between two activities"
date: 2020-07-12 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```rb
dependencies {
    ...
    implementation 'org.greenrobot:eventbus:3.2.0'
    ...
}
```

### NotificationData.kt

Data which needs to be sent. You can create your own data class.

```kotlin
data class NotificationData(
    val body: String,
    val clickAction: String,
    val title: String
)
```

### MyFirebaseMessagingService.kt - Sender 

```kotlin
MyFirebaseMessagingService: FirebaseMessagingService() {

    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)
        val messageText = message.notification?.body ?: message.data.get("body") ?: "No Message Body"
        var clickAction = message.notification?.clickAction ?: ""
        val title = message.notification?.title ?: "Notification"
        EventBus.getDefault().post(NotificationData(messageText, clickAction, title))
    }

}
```

### MainActivity.kt - Receiver

```kotlin
class MainActivity : AppCompatActivity() {
    private var isAppInForeground = false
    override fun onCreate() {
        super.onCreate()
        EventBus.getDefault().register(this)
    }

    override fun onDestroy() {
        super.onDestroy()
        EventBus.getDefault().unregister(this)
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    fun onThrowEvent(data : NotificationData) {
        if (data.clickAction.isNotEmpty()) {
            val snack = Snackbar.make(
                this@MainActivity.containerView,
                data.body,
                Snackbar.LENGTH_LONG
            )
            snack.setAction("Dismiss", View.OnClickListener {
                println("User pressed Dismiss button on snackbar")
            })
            snack.show()
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```