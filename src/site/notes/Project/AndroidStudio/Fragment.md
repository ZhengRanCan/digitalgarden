---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Fragment/","dgPassFrontmatter":true}
---

# 使用

## 添加依赖
 ```kotlin
 dependencies {
    implementation("androidx.fragment:fragment:1.2.0")  // 请使用最新版本
}
 ```
## 创建Fragment类
```kotlin
class MyFragment : Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater, 
        container: ViewGroup?, 
        savedInstanceState: Bundle?
    ): View {
        return ComposeView(requireContext()).apply {
            // 设置Compose内容
            setContent {
                Greeting(name = "Compose")
            }
        }
    }
}
```
## 将Fragment添加到Activity
```kotlin
// Kotlin代码示例
class MyActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_my)

        // 检查是否已经创建了Fragment
        if (savedInstanceState == null) {
            // 创建Fragment实例
            val myFragment = MyFragment()

            // 使用FragmentManager和FragmentTransaction添加Fragment
            supportFragmentManager.beginTransaction().apply {
                replace(R.id.fragment_container, myFragment)
                commit()
            }
        }
    }
}
```
## Fragment Manager
FragmentManager 类负责在应用的 fragment 上执行一些操作，如添加、移除或替换操作，以及将操作添加到返回堆栈。
如果用的是Jetpack Navigation，就需要与Fragment直接交互。
1. FragmentActivity 及其子类（如 AppCompatActivity都可以通过 `getSupportFragmentManager()` 方法访问 `FragmentManager`。
2. 在Fragment中，可以通过 `getChildFragmentManager` 获取对管理fragment子级的fragmentManager的引用
![Pasted image 20240526142643.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240526142643.png)
## FragmentTransaction
作为一个单元提交给FragmentManager
- **添加Fragment**：使用`add()`方法将Fragment添加到Activity的容器中。
- **移除Fragment**：使用`remove()`方法将Fragment从Activity中移除。
- **替换Fragment**：使用`replace()`方法替换容器中的Fragment。
- **Fragment事务回退**：通过`addToBackStack()`方法，可以将事务添加到返回栈中，允许用户通过按返回键来撤销Fragment事务。
- **执行事务**：使用`commit()`方法来提交事务，这表明所有的操作都已经添加到事务中，并且准备执行。

真用Fragment吗？真的假的，到现在都还是保持疑问啊
# REF:
1. 