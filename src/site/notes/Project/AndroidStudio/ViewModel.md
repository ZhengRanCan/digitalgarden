---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/ViewModel/","dgPassFrontmatter":true}
---

# 介绍
视图与数据之间进行数据交互的桥梁
使用 Jetpack Compose 时，ViewModel 是向可组合项公开屏幕界面状态的主要方式。


# 使用
## 创建ViewModel类
```kotlin
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class MyViewModel : ViewModel() {
	//MutableLiveData允许更新更新数据
    private val _text = MutableLiveData<String>()
    //LiveData只读
    val text: LiveData<String> get() = _text

    init {
        _text.value = "Hello, ViewModel!"
    }

    fun updateText(newText: String) {
        _text.value = newText
    }
}
```
### 注意点：
1. 关于val变量明明是只读变量，但是值还是能够变化
![[Component/components-20240523145926.components]]
## 调用Viewmodel
```kotlin
import android.os.Bundle
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.Observer

class MyActivity : AppCompatActivity() {
	//创建Viewmodel实例
    private val viewModel: MyViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        // 观察 LiveData 对象
        viewModel.text.observe(this, Observer { newText ->
            // 更新UI，例如设置TextView的文本
            findViewById<TextView>(R.id.myTextView).text = newText
        })
        // 假设有个按钮可以更新文本
        findViewById<Button>(R.id.myButton).setOnClickListener {
            viewModel.updateText("New text from button click")
        }
    }
}

```
## [[Project/AndroidStudio/依赖注入\|依赖注入]]
在Android架构中，ViewModel用来管理与UI相关的数据，当ViewModel需要依赖外部资源（如数据存储，网络访问时），通过构造函数注入依赖
### Hilt
```kotlin
//1.设置Hilt
@HiltAndroidApp
class MyApplication : Application()

//2.创建需要注入的依赖
class MyRepository @Inject constructor(
    private val apiService: ApiService,
    private val database: MyDatabase
) {
    // Repository implementation
}

//3.创建Viewmodel并注入Repository
@HiltViewModel
class MyViewModel @Inject constructor(
    private val repository: MyRepository
) : ViewModel() {
    private val _data = MutableLiveData<String>()
    val data: LiveData<String> get() = _data
    init {
        loadData()
    }
    private fun loadData() {
        // Use repository to load data
        // Example: _data.value = repository.getData()
    }
}

//4.使用 ViewModel 在 Activity
@AndroidEntryPoint
class MyActivity : AppCompatActivity() {
    private val viewModel: MyViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        viewModel.data.observe(this, Observer { data ->
            // Update UI with data
            findViewById<TextView>(R.id.myTextView).text = data
        })
    }
}

//5.配置Hilt模块
@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    fun provideApiService(): ApiService {
        // Provide ApiService implementation
    }

    @Provides
    fun provideDatabase(@ApplicationContext context: Context): MyDatabase {
        // Provide Database implementation
    }
}
```

# ViewModel中的数据格式
## LiveData或StateFlow
1. LiveData是一种生命周期感知的数据持有类，常用于传统的Android开发。它在数据变化时通知观察者，并在适当的时候自动停止通知。
2.  `StateFlow` 是一种基于 Kotlin 流的状态管理方式，适用于更现代的协程 API 和 Jetpack Compose。
```kotlin
//LiveData
class ReportViewModel : ViewModel() {
    private val _reports = MutableLiveData<List<ReportData>>()
    val reports: LiveData<List<ReportData>> = _reports

    fun setReports(newReports: List<ReportData>) {
        _reports.value = newReports
    }
}
//StateFlow
class ReportViewModel : ViewModel() {
    private val _reports = MutableStateFlow<List<ReportData>>(emptyList())
    val reports: StateFlow<List<ReportData>> = _reports

    fun setReports(newReports: List<ReportData>) {
        _reports.value = newReports
    }
}
```
## Compose的State
在 Jetpack Compose 中，你可以使用 `mutableStateOf` 或 `mutableStateListOf` 来管理简单的状态。
```kotlin
class ReportViewModel : ViewModel() {
    var reports by mutableStateOf<List<ReportData>>(emptyList())
        private set

    fun setReports(newReports: List<ReportData>) {
        reports = newReports
    }
}

```
## 使用Room Database
如果需要持久性数据，可以使用Room数据库，Room是一个基于SQLite的对象映射库
```kotlin
//定义实体类
@Entity
data class ReportData(
    @PrimaryKey val id: Int,
    val patient: String,
    val analysis: String,
    val report: String,
    val time: String
)
//创建DAO接口
@Dao
interface ReportDao {
    @Query("SELECT * FROM ReportData")
    fun getAllReports(): Flow<List<ReportData>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertReports(reports: List<ReportData>)
}
//创建数据库类
@Database(entities = [ReportData::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun reportDao(): ReportDao
}
//使用ViewModel
class ReportViewModel(application: Application) : AndroidViewModel(application) {
    private val database = Room.databaseBuilder(application, AppDatabase::class.java, "reports-db").build()
    private val reportDao = database.reportDao()

    val reports: Flow<List<ReportData>> = reportDao.getAllReports()

    fun setReports(newReports: List<ReportData>) {
        viewModelScope.launch {
            reportDao.insertReports(newReports)
        }
    }
}
```
## Repository
使用 Repository 模式可以更好地分离数据逻辑和业务逻辑。这种模式通常与 `LiveData`、`Flow` 或 Room 数据库结合使用。
```kotlin
class ReportRepository(private val reportDao: ReportDao) {
    val reports: Flow<List<ReportData>> = reportDao.getAllReports()

    suspend fun setReports(newReports: List<ReportData>) {
        reportDao.insertReports(newReports)
    }
}

class ReportViewModel(private val repository: ReportRepository) : ViewModel() {
    val reports: Flow<List<ReportData>> = repository.reports

    fun setReports(newReports: List<ReportData>) {
        viewModelScope.launch {
            repository.setReports(newReports)
        }
    }
}

```
## Parcelable
`Parcelable` 允许数据类在 Android 组件（如 `Intent`、`Bundle` 等）之间传递数据。
在保存和恢复 Activity 状态时，`Parcelable` 对象非常便于使用。
```kotlin
@Parcelize
data class ReportData(
    val patient: String = "郑栩轩",
    val analysis: String = "这是舌象分析",
    val report: String = "这是舌象报告",
    val time: String
) : Parcelable
```
### 总结
1. **`LiveData` 和 `StateFlow`**：适用于需要观察和响应数据变化的场景。
2. **Compose 的 `mutableStateOf` 和 `mutableStateListOf`**：适用于 Jetpack Compose 中的简单状态管理。
3. **Room Database**：适用于需要持久化数据的场景。
4. **Repository 模式**：适用于需要更好地分离数据逻辑和业务逻辑的场景。
# REF：
1. [ViewModel 概览  |  Android 开发者  |  Android Developers](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=zh-cn)
2. [[Project/AndroidStudio/Kotlin变量、函数、类型\|Kotlin变量、函数、类型]]
3. [[Project/AndroidStudio/Data Class与Class的区别\|Data Class与Class的区别]]