---
layout: post
title: "Android - Kotlin - RecyclerView Example"
date: 2020-07-03 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```
dependencies {
    ...
    implementation 'androidx.recyclerview:recyclerview:1.1.0'
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
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        listView.adapter = MainListAdapter(listOf<ListItem>(ListItem("Sagar", "Kothari")))
    }
}
```