---
layout: post
title: "Android - Firebase Push notifications - Foreground Snackbar, Background NotificationManager"
date: 2020-07-14 07:20:00 +0530
categories: Android Kotlin
---

Before we read this, please make sure you've completed reading following articles.
- [Event Bus in Android](https://sagar-r-kothari.github.io/android/kotlin/2020/07/12/Anroid-EventBus.html)
- [Background Check](https://sagar-r-kothari.github.io/android/kotlin/2020/07/11/Anroid-Background-Foreground-Check.html)

### build.gradle (Module: app)

```rb
dependencies {
    ...
    implementation 'androidx.lifecycle:lifecycle-process:2.2.0'
    implementation 'org.greenrobot:eventbus:3.2.0'
    ...
}
```

### Step 1: MyFirebaseMessagingService - Observe Foreground / Background

Add LifecycleObserver to your `MyFirebaseMessagingService` class.

```kotlin
class MyFirebaseMessagingService : FirebaseMessagingService(), LifecycleObserver {
    private var isAppInForeground = false
    override fun onCreate() {
        super.onCreate()
        ProcessLifecycleOwner.get().lifecycle.addObserver(this)
    }

    override fun onDestroy() {
        super.onDestroy()
        ProcessLifecycleOwner.get().lifecycle.removeObserver(this)
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun onForegroundStart() {
        isAppInForeground = true
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun onForegroundStop() {
        isAppInForeground = false
    }
}
```

### Step 2. Data Class for notification object.

```kotlin
data class NotificationData(
    val body: String,
    val clickAction: String,
    val title: String
)
```

### Step 3: MyFirebaseMessagingService onMessageReceived - Send data using EventBus if App is in foreground

Check - If App is running & on forground, pass data using event-bus.
Only If app is in background / killed, show notification

```kotlin
// MyFirebaseMessagingService.kt

    override fun onMessageReceived(message: RemoteMessage) {
        super.onMessageReceived(message)

        // Push Notification object MUST NOT contain `notification` for Android
        // If it has `notification` object, controll will pass to Android.
        // and when app is killed our App won't get control.
        // So, server/platform must send push-notification using `data` object 
        // and must avoid `notification`


        // We MUST READ values from Data
        val messageText = message.data.get("body") ?: "No Message Body"
        var clickAction = message.data.get("click_action") ?: ""
        val title = message.data.get("title") ?: "MyApplication"
        val components = clickAction.split("?type=")
        clickAction = if (components.count() == 2) components[1] else ""
        if(isAppInForeground && clickAction.isNotBlank()) {
            EventBus.getDefault().post(NotificationData(messageText, clickAction, title))
        } else {
            sendNotification(NotificationData(messageText, clickAction, title))
        }
    }
```

### Step 4: Receive Data from Event Bus if App is in foreground.

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onStart() {
        super.onStart()
        EventBus.getDefault().register(this)
    }

    override fun onStop() {
        super.onStop()
        EventBus.getDefault().unregister(this)
    }

    @Subscribe(threadMode = ThreadMode.MAIN)
    fun onThrowEvent(data : NotificationData) {
        // your preferred set of actions here.
        // Here I'm showing a snackbar.
        if (data.clickAction.isNotEmpty()) {
            val snack = Snackbar.make(
                this@MainActivity.containerView, // Use your view to show Snackbar
                data.body,
                Snackbar.LENGTH_LONG
            )
            snack.setAction("Open", View.OnClickListener {
                // Do something when user taps on open
            })
            snack.show()
        }
    }
}
```

### Step 5: Send notification if app is in background

```kotlin
// MyFirebaseMessagingService.kt

    private fun sendNotification(data: NotificationData) {
        // 1. Get Intent of activity which you want to be launched
        val intent = Intent(this.applicationContext, MainActivity::class.java)
        // 2. Add Flags
        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
        intent.addFlags(Intent.FLAG_INCLUDE_STOPPED_PACKAGES)
        // 3. Add Extras
        intent.putExtra("message", data.body)
        intent.putExtra("clickAction", data.clickAction)

        // 4. Create Pending Intent (Intent yet to launch) with above intent
        val pendingIntent = TaskStackBuilder.create(this)
             .addNextIntentWithParentStack(intent)
             .getPendingIntent(0, PendingIntent.FLAG_ONE_SHOT)

        // 5. Default sound for notification.
        val defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)

        // 6. Create NotificationCompact Object
        val notificationBuilder = NotificationCompat.Builder(this, "ChannelID")
            .setSmallIcon(R.mipmap.ic_launcher_round) // 6.1 Set Small Icon - MUST
            .setLargeIcon(BitmapFactory.decodeResource(resources, R.mipmap.ic_launcher_round)) // 6.2 Set Large Icon - optional
            .setContentTitle(data.title) // 6.3 Set Content Title - MUST
            .setContentText(data.body) // 6.3 Set Content Text - MUST
            .setAutoCancel(true) // 6.4 notification is automatically canceled when the user clicks it in the panel
            .setSound(defaultSoundUri) // 6.5 Set sound
            .setContentIntent(pendingIntent) // 6.6 set pending intent - IMPORTANT

        // 7. With notification manager, send above notification using above object object
        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel("MyApplication",
                "MyApplication Push Notifications",
                NotificationManager.IMPORTANCE_DEFAULT)
            notificationManager.createNotificationChannel(channel)
        }
        notificationManager.notify(0, notificationBuilder.build())
    }
```

### 6. Read data from Notification when app was killed but launched using Notification.

```kotlin
// MainActivity.kt

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val clickAction: String? = intent.extras?.get("clickAction") as String?
        val message: String? = intent.extras?.get("message") as String?
    }
```