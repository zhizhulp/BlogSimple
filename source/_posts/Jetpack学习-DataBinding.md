---
title: Jetpack学习-DataBinding
date: 2019-06-04 11:52:23
tags:
---

##### BindingMethods

```kotlin
@BindingMethods(value = [
    BindingMethod(
        type = android.widget.ImageView::class,
        attribute = "android:tint",
        method = "setImageTintList")])
```

将ImageView的已有属性 android:tint（可以改变图片颜色，不用每个状态都p一张图片） 与 [setImageTintList(ColorStateList)](https://developer.android.google.cn/reference/android/widget/ImageView.html#setImageTintList(android.content.res.ColorStateList)) 关联起来，而不是和setTint()（这个方法不存在）关联起来。BindingMethods的作用就是将 **存在的属性** 和 **存在但是不对应的方法** 关联起来。

##### BindingAdapter

```kotlin
@BindingAdapter("android:paddingLeft")
fun setPaddingLeft(view: View, padding: Int) {
    view.setPadding(padding,
                view.getPaddingTop(),
                view.getPaddingRight(),
                view.getPaddingBottom())
}
```

View中的属性 android:paddingLeft 在代码中没有与之对应的（View中没有setPaddingLeft()），但是View中有setPadding(left,top,right,bottom)。BindingAdapter的作用就是将 **已存在的属性** 和 **自定义的方法** 关联起来。

BindingAdapter可以包含多个属性,下面是演示Picasso加载网络图片的方法：

```
@BindingAdapter("imageUrl", "error")
fun loadImage(view: ImageView, url: String, error: Drawable) {
    Picasso.get().load(url).error(error).into(view)
}
```

布局中这样写

```xml
<ImageView
	app:imageUrl="@{venue.imageUrl}"
	app:error="@{@drawable/venueError}"/>
```

上面的BindingAdapter有2个参数，如果你想其中任何一个参数改变都会触发方法，你可以这样写：

```kotlin
@BindingAdapter(value = ["imageUrl", "error"]，requireAll = false)
fun loadImage(view: ImageView, url: String, error: Drawable) {
    Picasso.get().load(url).error(error).into(view)
}
```

使用@get:Bindable无法生成BR文件的解决办法

```
apply plugin: 'kotlin-kapt'
```

BindingAdapter有个很重要的作用，举例说明：

```kotlin
data class User(val name:String)
```

```xml
<TextView
	android:text="@{user.name}"/>
```

```
val user = User("xiaoli")
```

正常来说binding框架会调用TextView.setText()方法，来显示结果即：xiaoli，但是我希望显示是大写的，那么可以这么写：

```kotlin
@BindingAdapter("android:text")
fun setText(TextView view, CharSequence text) {
    val newText = text.toString().toUpercase()
    view.text = newText;
}
```

总结来说

当在任意一个View的任意一个属性上使用binding表达式时，DataBinding框架的处理过程分成三步：
 1、对binding表达式求值
 2、寻找合适的BindingAdapter，如果找到，就调用它的方法
 3、如果没有找到合适的BindingAdapter，就在View上寻找合适的方法调用