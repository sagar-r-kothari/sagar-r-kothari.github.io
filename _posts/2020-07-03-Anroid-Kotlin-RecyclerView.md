---
layout: post
title: "Android - Kotlin - RecyclerView Example"
date: 2020-07-03 07:20:00 +0530
categories: Android Kotlin
---

### build.gradle (Module: app)

```rb
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
    private val list: List<ListItem>,
    private val onItemClicked: (Int) -> Unit,
): RecyclerView.Adapter<MainListAdapter.ViewHolder>() {

    class ViewHolder(
        itemView: View,
        val onItemClicked: (Int
    ) -> Unit): RecyclerView.ViewHolder(view) {
        fun bindItems(item: ListItem, position: Int) {
            itemView.title.text = list[position].title
            itemView.subtitle.text = list[position].subtitle
            Glide.with(holder.itemView)  // glide with this view
                .load(list[position].subtitle) // download using this url
                .override(250, 250) // set this size
                .placeholder(R.mipmap.ic_launcher) // default placeHolder image
                .error(R.mipmap.ic_launcher) // use this image if faile
                .into(holder.itemView.imageView) // set
            itemView.setOnClickListener {
                Log.d("MenuItem", "Clicked on $position")
                this.onItemClicked(position)
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MainListAdapter.ViewHolder {
        val v = LayoutInflater.from(parent.context).inflate(R.layout.list_item, parent, false)
        return ViewHolder(v, onItemClicked)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bindItems(list[position], position)
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
        listView.adapter = MainListAdapter(listOf<ListItem>(ListItem("Sagar", "Kothari"))) { index ->
            Log.d("List", "Tapped at index $index")
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