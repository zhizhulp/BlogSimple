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

