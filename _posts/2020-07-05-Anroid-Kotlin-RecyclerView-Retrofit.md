---
layout: post
title: "Android - Kotlin - RetroFit RecyclerView, Pull to load more, pull to refresh"
date: 2020-07-05 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```
dependencies {
    ...
    implementation 'androidx.recyclerview:recyclerview:1.1.0'
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0'
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
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

### GetUsersService

```kotlin
data class User(
    @SerializedName("login") val login: String?,
    @SerializedName("id") val id: Int?,
    @SerializedName("avatar_url") val avatarUrl: String?
)

class GetUsersService {

    interface GitHubService {
        @GET("users?")
        fun listRepos(@Query("since") since: Int?): Call<List<User?>?>
    }

    private val baseUrl = "https://api.github.com/"
    private val retrofit= Retrofit.Builder()
        .baseUrl(baseUrl)
        .addConverterFactory(GsonConverterFactory.create())
        .build()
    private val service = retrofit.create(GitHubService::class.java)
    private var lastId: Int? = null

    private fun fetchData(value: Int?, onResult: (List<User?>?) -> Unit) {
        val call = service.listRepos(value)
        call.enqueue(
            object : Callback<List<User?>?> {
                override fun onFailure(call: Call<List<User?>?>, t: Throwable) {
                    onResult(null)
                }
                override fun onResponse(call: Call<List<User?>?>, response: Response<List<User?>?>) {
                    println("List users are ${response.body()}")
                    lastId = response.body()?.last()?.id
                    onResult(response.body())
                }
            }
        )
    }

    fun users(onResult: (List<User?>?) -> Unit) {
        fetchData(null, onResult)
    }

    fun nextPage(onResult: (List<User?>?) -> Unit) {
        if (lastId == null) {
            onResult(null)
            return
        }
        fetchData(lastId, onResult)
    }
}
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    val usersService = GetUsersService()
    var list = mutableListOf<ListItem>(ListItem("Sagar", "Kothari"))

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        listView.adapter = MainListAdapter(list)
        swipeRefresh.setOnRefreshListener {
            getUsers()
        }
        getUsers()
        listView.addOnScrollListener(object : RecyclerView.OnScrollListener() {
            override fun onScrollStateChanged(recyclerView: RecyclerView, newState: Int) {
                super.onScrollStateChanged(recyclerView, newState)
                if (!recyclerView.canScrollVertically(1) && newState == RecyclerView.SCROLL_STATE_IDLE) {
                    loadMoreUsers()
                }
            }
        })
    }

    private fun loadMoreUsers() {
        swipeRefresh.isRefreshing = true
        usersService.nextPage {
            if (it != null) {
                val newTransformed = it
                    .filter { it != null && it.login != null && it.avatarUrl != null }
                    .map { ListItem(it!!.login!!, it.avatarUrl!!) }
                list.addAll(newTransformed)
                (listView.adapter as MainListAdapter).notifyDataSetChanged()
            }
            swipeRefresh.isRefreshing = false
        }
    }

    private fun getUsers() {
        swipeRefresh.isRefreshing = true
        listView.adapter = MainListAdapter(listOf<ListItem>())
        usersService.users {
            swipeRefresh.isRefreshing = false
            if (it != null) {
                list = it
                    .filter { it != null && it.login != null && it.avatarUrl != null }
                    .map { ListItem(it!!.login!!, it.avatarUrl!!) } as MutableList<ListItem>
                listView.adapter = MainListAdapter(list)
            }
        }
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