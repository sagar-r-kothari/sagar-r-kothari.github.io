---
layout: post
title: "Android - Kotlin - RecyclerView with Pull to refresh with Volley Example"
date: 2020-07-04 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```rb
dependencies {
    ...
    implementation 'androidx.recyclerview:recyclerview:1.1.0'
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0'
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.google.code.gson:gson:2.8.5'
    implementation 'com.github.bumptech.glide:glide:4.9.0'
    ...
}
```

### List Item

```kotlin
data class ListItem (
    val title: String,
    val subtitle: String
)
```

### MainListAdaptor.kt

```kotlin
class MainListAdapter (
    private val list: List<ListItem>
): RecyclerView.Adapter<MainListAdapter.ViewHolder>() {

    class ViewHolder(view: View): RecyclerView.ViewHolder(view)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MainListAdapter.ViewHolder {
        return ViewHolder(
            LayoutInflater
                .from(parent.context)
                .inflate(R.layout.list_main, parent, false)
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.itemView.title.text = list[position].title
        holder.itemView.subtitle.text = list[position].subtitle
        Glide.with(holder.itemView)  // glide with this view
            .load(list[position].subtitle) // download using this url
            .override(250, 250) // set this size
            .placeholder(R.mipmap.ic_launcher) // default placeHolder image
            .error(R.mipmap.ic_launcher) // use this image if faile
            .into(holder.itemView.imageView) // set
    }

    override fun getItemCount(): Int {
        return list.size
    }
}
```

### MainActivity.kt

```kotlin
class User(
    val login: String,
    val id: Int,
    val avatar_url: String
)

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        listView.adapter = MainListAdapter(listOf<ListItem>(ListItem("Sagar", "Kothari")))
        swipeRefresh.setOnRefreshListener {
            currentURL = "https://api.github.com/users"
            getUsers()
        }
        getUsers()
    }

    fun getUsers() {
        swipeRefresh.isRefreshing = true
        listView.adapter = MainListAdapter(listOf<ListItem>())
        val queue = Volley.newRequestQueue(this)
        val arrayUsers = object : TypeToken<Array<User>>() {}.type
        val stringRequest = StringRequest(Request.Method.GET, currentURL, Response.Listener {
            val gson = Gson()
            val arrayUsers = object : TypeToken<Array<User>>() {}.type
            var users: Array<User> = gson.fromJson(it,  arrayUsers)
            val list = users.map { ListItem(it.login, it.avatar_url) }
            listView.adapter = MainListAdapter(list)
            swipeRefresh.isRefreshing = false
            val link = request?.headers?.get("link") ?: ""
            if (link.isNotEmpty()) {
                println("Link is $link")
            }
        }, Response.ErrorListener {
            println("MyCardsHolder - Failed Getting users")
            Toast.makeText(this, "Something went wrong. Please try again.", Toast.LENGTH_SHORT)
            swipeRefresh.isRefreshing = false
        })
        stringRequest
        request = stringRequest
        queue.add(stringRequest)
    }
}
```

### activity_main.xml


```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager" />

</androidx.constraintlayout.widget.ConstraintLayout>
```