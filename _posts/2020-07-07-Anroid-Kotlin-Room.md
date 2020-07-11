---
layout: post
title: "Android - Kotlin - Room"
date: 2020-07-07 07:20:00 +0530
categories: Android Kotlin
---

#### Preview

![Preview Image](/assets/andoid/name-room-kotlin-list.png)
![Preview Image](/assets/andoid/name-room-kotlin-add.png)

- Q: What are we trying to achieve here?
- A: We're trying to store & show list of Names.

### AndroidManifest.xml

Let's add permissions first.

```xml
<uses-permission
        android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission
        android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### build.gradle (Module: app)

2. And time to add dependencies.

```
apply plugin: 'kotlin-kapt'
dependencies {
    ...
    // UI Component
    implementation 'androidx.recyclerview:recyclerview:1.1.0'

    // Material design
    implementation "com.google.android.material:material:1.1.0"

    // LifeStyle Components
    implementation "androidx.lifecycle:lifecycle-extensions:2.2.0"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.2.0"
    kapt "androidx.lifecycle:lifecycle-compiler:2.2.0"

    // Kotlin Components
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    api "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.4"
    api "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.4"

    // Room Components
    implementation "androidx.room:room-runtime:2.2.5"
    kapt "androidx.room:room-compiler:2.2.5"
    implementation "androidx.room:room-ktx:2.2.5"
    ...
}
```

### ListItem.kt

List item data class

```kotlin
data class ListItem (
    val title: String,
    val subtitle: String
)
```

### MainListAdapter.kt

Create Adapter for RecyclerView List - which will display list of words.


```kotlin
class MainListAdapter : RecyclerView.Adapter<MainListAdapter.ViewHolder>() {
    private var list: List<ListItem> = emptyList()

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
        holder.itemView.setOnClickListener {
            println("Clicked on position $position")
        }
        println("Recycling view - $position")
    }

    override fun getItemCount(): Int {
        return list.size
    }

    internal fun setListItems(items: List<ListItem>) {
        this.list = items
        notifyDataSetChanged()
    }
}
```

### list_main.xml

- Layout file for single row - which is used in above adapter.
- ImageView we'll be using in next article.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:focusable="true"
    android:clickable="true"
    android:foreground="?android:attr/selectableItemBackground">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="5dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@mipmap/ic_launcher" />

    <LinearLayout
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="5dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/imageView"
        app:layout_constraintTop_toTopOf="@+id/imageView">

        <TextView
            android:id="@+id/title"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Sagar"
            android:textAppearance="@style/TextAppearance.AppCompat.Medium" />

        <TextView
            android:id="@+id/subtitle"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Kothari"
            android:textAppearance="@style/TextAppearance.AppCompat.Body1" />
    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

### NameTable.kt

Create table. For this article, we're creating name table.

```kotlin
import androidx.room.ColumnInfo
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "names_table")
class Name(
    @PrimaryKey(autoGenerate = true) val id: Int? = null,
    @ColumnInfo(name = "name") val name: String
)
```

### NameDAO.kt

Data Access Object - An interface - with which we'll be able to access Name table.

```kotlin
import androidx.lifecycle.LiveData
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.OnConflictStrategy
import androidx.room.Query

@Dao
interface NameDao {
    @Query("SELECT * from names_table ORDER BY name ASC")
    fun getAlphabetizedNames(): LiveData<List<Name>>

    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(name: Name)

    @Query("DELETE FROM names_table")
    suspend fun deleteAll()
}
```

### NameRepository.kt

Repository with which we'll be able to insert / delete / update / read.

```kotlin
import androidx.lifecycle.LiveData

class NameRepository(private val nameDao: NameDao) {

    val allNames: LiveData<List<Name>> = nameDao.getAlphabetizedNames()

    suspend fun insert(name: Name) {
        nameDao.insert(name)
    }
}
```

### NameRootDatabase.kt

Database connection

```kotlin
import android.content.Context
import androidx.room.Database
import androidx.room.Room
import androidx.room.RoomDatabase

@Database(entities = [Name::class], version = 1, exportSchema = false)
public abstract class NameRoomDatabase : RoomDatabase() {

    abstract fun nameDao(): NameDao

    companion object {
        @Volatile
        private var INSTANCE: NameRoomDatabase? = null

        fun getDatabase(context: Context): NameRoomDatabase {
            val tempInstance = INSTANCE
            if (tempInstance != null) {
                return tempInstance
            }
            synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    NameRoomDatabase::class.java,
                    "name_database"
                ).build()
                INSTANCE = instance
                return instance
            }
        }
    }
}
```

### NameViewModel.kt

ViewModel for async, easy, quick access to Name table.

```kotlin
import android.app.Application
import androidx.lifecycle.AndroidViewModel
import androidx.lifecycle.LiveData
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

class NameViewModel(application: Application) : AndroidViewModel(application) {

    private val repository: NameRepository
    val allNames: LiveData<List<Name>>

    init {
        val namesDao = NameRoomDatabase.getDatabase(application).nameDao()
        repository = NameRepository(namesDao)
        allNames = repository.allNames
    }

    fun insert(name: Name) = viewModelScope.launch(Dispatchers.IO) {
        repository.insert(name)
    }
}
```

### activity_main.xml

Layout for MainActivity

```xml
<androidx.constraintlayout.widget.ConstraintLayout
xmlns:android="http://schemas.android.com/apk/res/android"
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

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="end|bottom"
        android:layout_margin="16dp"
        android:src="@drawable/ic_add"
        app:layout_constraintBottom_toBottomOf="@id/listView"
        app:layout_constraintEnd_toEndOf="@id/listView"
        app:rippleColor="@color/colorPrimary" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### activity_additem.xml

Layout for AddItemActivity

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <EditText
        android:id="@+id/editTextWord"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPersonName"
        android:layout_margin="10dp"
        android:autofillHints="@string/enter_name"
        android:hint="@string/enter_name"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/saveButton"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="@string/save_button"
        android:layout_margin="10dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var nameViewModel: NameViewModel
    private val newNameActivityRequestCode = 1

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val listAdapter = MainListAdapter()
        listView.adapter = listAdapter

        nameViewModel = ViewModelProvider(this).get(NameViewModel::class.java)
        nameViewModel.allNames.observe(this, Observer { names ->
            names?.let { listAdapter.setListItems(it.map { ListItem(it.name, "ID: ${it.id}") }) }
        })

        fab.setOnClickListener {
            startActivityForResult(
                Intent(this@MainActivity, AddItemActivity::class.java),
                newNameActivityRequestCode)
        }
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        val text = data?.getStringExtra(AddItemActivity.EXTRA_REPLY)
        if (newNameActivityRequestCode == requestCode && resultCode == Activity.RESULT_OK && text != null) {
            nameViewModel.insert(Name(null, text!!))
        } else {
            Toast.makeText(applicationContext, R.string.save_error, Toast.LENGTH_LONG)
        }
    }
}
```

### AddItemActivity.kt

```kotlin
class AddItemActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_additem)
        saveButton.setOnClickListener {
            val replyIntent = Intent()
            if (TextUtils.isEmpty(editTextWord.text)) {
                setResult(Activity.RESULT_CANCELED, replyIntent)
            } else {
                replyIntent.putExtra(EXTRA_REPLY, editTextWord.text.toString())
                setResult(Activity.RESULT_OK, replyIntent)
            }
            finish()
        }
    }

    companion object {
        const val EXTRA_REPLY = "com.sagar_r_kothari.mycardsholder.AdditemActivity.Reply"
    }
}
```