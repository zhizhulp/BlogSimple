---
title: Jetpack学习-Lifecycles
date: 2019-06-03 10:35:53
tags: [Android，Jetpack]
categories: Jetpack
---

##### Lifecycle为何出现？

1. 在Activity生命周期方法中例如onCreate()、onStart()、onStop()中有大量的代码，例如一个获取用户位置的组件需要在onCreate()中初始化实例，在onStart()中开始获取用户位置，在onStop()中停止获取用户位置。这只是一个组件，在实际开发当中，业务是复杂的，组件也是很多的，维护这么多组件，会耗费很多精力。

2. 是我们无法预测组件能在onStop()之前开始。例如一个定位组件需要在onStart()中检查配置（耗时操作），然后回调结果，开始获取位置。但是用户按下了home键，此时app进入后台，onStart()收到回调，那么在回调中做一些操作就会很危险（[官方翻译](https://developer.android.google.cn/topic/libraries/architecture/lifecycle) 是组件保持活动的时间超过其所需的时间，大概意思是本应该在onStop()中销毁的一些对象，重新在onStart收到回调后复活了）。

##### 如何处理on stop events?

Event.ON_STOP 并不是随着Activity生命周期onStop()调用的，而是伴随着Activity生命周期中的onSaveInstanceState()，onStop()是在onSaveInstanceState()后面调用的。下面是[demo]( https://github.com/zhizhulp/JetPackDemo.git)中的log：


```
//进入MainActivity
2019-06-03 18:29:51.455 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onCreate
2019-06-03 18:29:51.487 9831-9831/com.example.jetpackdemo D/LocationM: create-CREATED
2019-06-03 18:29:51.488 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onStart
2019-06-03 18:29:51.489 9831-9831/com.example.jetpackdemo D/LocationM: start-STARTED
2019-06-03 18:29:51.493 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onResume
2019-06-03 18:29:51.494 9831-9831/com.example.jetpackdemo D/LocationM: resume-RESUMED
//按home键/进入其他页面/按电源键/横竖屏切换（即app短暂进入后台）
2019-06-03 18:29:56.099 9831-9831/com.example.jetpackdemo D/LocationM: pause-STARTED
2019-06-03 18:29:56.100 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onPause
2019-06-03 18:29:56.522 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onSaveInstanceState
2019-06-03 18:29:56.523 9831-9831/com.example.jetpackdemo D/LocationM: stop-CREATED
2019-06-03 18:29:56.524 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onStop
//重新进入MainActivity
2019-06-03 18:30:07.496 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onStart
2019-06-03 18:30:07.497 9831-9831/com.example.jetpackdemo D/LocationM: start-STARTED
2019-06-03 18:30:07.497 9831-9831/com.example.jetpackdemo D/MainActivity_LocationM: onResume
2019-06-03 18:30:07.500 9831-9831/com.example.jetpackdemo D/LocationM: resume-RESUMED
```

##### 如何理解State?

官方的图解如下:

![/source/images](\images\lifecycle-states.png)

为什么State只有5个？分别为INITIALIZED、DESTROYED、CREATED、STARTED、RESUMED，这里我参考了 [这个博客](https://blog.csdn.net/u012124438/article/details/88719321) （非常感谢），其实这5个状态并不像字面上的意思。

1. 生命周期状态为RESUMED时表示，当前activity 是在前台，并且可交互也就onResume()执行后。
2. 生命周期状态为STARTED时，表示当前activity处于可见但是不可交互，也就是onStart()方法刚执行完或者onPause()方法刚执行完的状态。
3. 生命周期状态为CREATED，表示onCreate()方法刚刚执行完或者onStop()方法刚刚执行完，也就是当前activity不在前台，但是也没有处于销毁状态。
4. 生命周期状态为DESTORYED,表示当前Activity还不存在，没有被创建或者已经销毁，我们通常考虑比较多的就是，onDestory()方法执行后，当前Activity已经销毁。
5. 生命周期状态为INITIALIZED，表示初始状态。

所以，如果我们要保证在Activity/Fragment的有效生命周期内进行的操作，必须判断，当前lifecycle的状态是否至少是CREATED状态，避免Activity或者fragment销毁了以后，回调或者网络请求才回来，此时做一些操作会导致异常。代码如下

```
if (lifecycle.currentState.isAtLeast(Lifecycle.State.STARTED)) {
	// do something 保证数据回调回来，当前activity是存在的，也就是State必须为STARTED、RESUMED，		   Activity此时的状态应该为onStart、onResume、onPause。
 }
```

