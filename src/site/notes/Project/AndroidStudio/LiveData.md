---
{"tags":["Project/AndroidStudio/Kotlin","Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/LiveData/","dgPassFrontmatter":true}
---

# 定义
一种可观察的数据存储类，具有生命周期感知能力，这可确保LiveData仅更新活跃**生命周期**的组件

---
# 使用
1. 创建LiveData的实例以存储某种类型的数据，通常在ViewModel类中完成。
2. 创建onChanged() 方法的Observer对象，该方法可以用于控制当LiveData对象数据发生数据，页面会发生什么
3. 使用observer () 方法将Observer附加到LiveData对象。
## 创建
```kotlin
class NameViewModel : ViewModel() {    
	// Create a LiveData with a String    
	val currentName: MutableLiveData<String> by lazy {
        MutableLiveData<String>()    
   }    
	// Rest of the ViewModel...  
}
```
1. LiveData对象通常存储在ViewModel对象，通过getter方法进行访问
2. 一般不在Activity或者Fragment上写
## 观察
```kotlin
class NameActivity : AppCompatActivity() {    
	// Use the 'by viewModels()' Kotlin property delegate    
	// from the activity-ktx artifact    
	private val model: NameViewModel by viewModels()
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)        
	    // Other code to setup the activity...        
	    // Create the observer which updates the UI.        
		val nameObserver = Observer<String> { newName ->            
	// Update the UI, in this case, a TextView.            
		  nameTextView.text = newName        
	}      
	// Observe the LiveData, passing in this activity as the LifecycleOwner and the observer.
	model.currentName.observe(this, nameObserver)    
	}  
}
```
1. 大多数情况下，用onCreated () 方法观察LIveData对象
2. 在传递 `nameObserver` 参数的情况下调用 [`observe()`](https://developer.android.com/reference/androidx/lifecycle/LiveData?hl=zh-cn#observe(androidx.lifecycle.LifecycleOwner,%0Aandroidx.lifecycle.Observer%3CT%3E)) 后，系统会立即调用 [`onChanged()`](https://developer.android.com/reference/androidx/lifecycle/Observer?hl=zh-cn#onChanged(T))，从而提供 `mCurrentName` 中存储的最新值。如果 `LiveData` 对象尚未在 `mCurrentName` 中设置值，系统不会调用 `onChanged()`。
## 更新
```kotlin
button.setOnClickListener {    
	val anotherName = "John Doe"    
	model.currentName.setValue(anotherName)  
}
```
1. LiveData 没有公开可用的方法来更新存储的数据。[`MutableLiveData`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData?hl=zh-cn) 类将公开 [`setValue(T)`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData?hl=zh-cn#setValue(T)) 和 [`postValue(T)`](https://developer.android.com/reference/androidx/lifecycle/MutableLiveData?hl=zh-cn#postValue(T)) 方法，如果需要修改存储在 [`LiveData`](https://developer.android.com/reference/androidx/lifecycle/LiveData?hl=zh-cn) 对象中的值，则必须使用这些方法。通常情况下会在 [`ViewModel`](https://developer.android.com/reference/androidx/lifecycle/ViewModel?hl=zh-cn) 中使用 `MutableLiveData`，然后 `ViewModel` 只会向观察者公开不可变的 `LiveData` 对象。


---
# 名词 ：
1. 生命周期：共有五种：created，destroyed，initalized，resumed，started
2. `by ` 关键字：用于属性委托，并将属性的getter和setter委托给另一个对象
# REF：
1. [LiveData 概览  |  Android 开发者  |  Android Developers](https://developer.android.com/topic/libraries/architecture/livedata?hl=zh-cn)