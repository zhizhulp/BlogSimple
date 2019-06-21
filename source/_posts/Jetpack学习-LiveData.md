---
title: 'Jetpack学习-LiveData'
date: 2019-06-20 09:44:45
tags:
---

##### LiveData的基本使用

1. 新建一个LiveData实例，通常在ViewModel中创建；
2. 建立一个Observer对象，覆写onChanged()；
3. 调用liveData.observer(LifeCycleOwner,Observer)；

代码如下

```xml
implementation 'androidx.lifecycle:lifecycle-extensions:2.0.0-beta01'
```

```kotlin
class NameViewModel : ViewModel() {
    // Create a LiveData with a String
    val currentName: MutableLiveData<String> by lazy {
        MutableLiveData<String>()
    }
    // Rest of the ViewModel...
}
```

```kotlin
class NameActivity : AppCompatActivity() {
    private lateinit var model: NameViewModel
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Other code to setup the activity...
        // Get the ViewModel.
        model = ViewModelProviders.of(this).get(NameViewModel::class.java)
        // Create the observer which updates the UI.
        val nameObserver = Observer<String> { newName ->
            // Update the UI, in this case, a TextView.
            nameTextView.text = newName
        }
        // Observe the LiveData, passing in this activity as the LifecycleOwner 			and the observer.
        model.currentName.observe(this, nameObserver)
    }
}
```

ViewModel会自动帮你管理生命周期，很智能：

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val viewModel =ViewModelProviders.of(this).get(MyViewModel::class.java)
        viewModel.user.value = User("张laoshi")
        viewModel.user.observe(this,Observer<User>{ user ->
            Log.d("LiveData","设置名字${user.name}")
            tv.text = user.name
        })
        /**
         * 假如在5s内按下home键，5s到了不会设置新的名字，如果切回当前activity，会立即设置新的名字
         */
        Handler().postDelayed(Runnable {
            Log.d("LiveData","5s到了。")
            viewModel.user.value = User("asd")
        },5000)
    }
}
class MyViewModel : ViewModel(){
    val user:MutableLiveData<User> by lazy {
        MutableLiveData<User>()
    }
}
data class User(var name:String)
```