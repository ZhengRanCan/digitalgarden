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
# REF：
1. [ViewModel 概览  |  Android 开发者  |  Android Developers](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=zh-cn)
2. [[Project/AndroidStudio/Kotlin变量、函数、类型\|Kotlin变量、函数、类型]]