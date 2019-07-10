---
title: Jetpack学习-ViewModel
date: 2019-06-24 09:19:53
tags:
---

##### 为什么需要ViewModel

ViewModel是以生命感知的方式，用于存储和管理与UI相关的数据。ViewModel允许在配置改变（例如设备旋转）的情况下依然保持数据的存活。

1. 传统的数据回复方式 onSaveInstanceState() 不适合恢复大量的数据，比如一个bitmap或用户列表。
2. UI controllers经常需要做一些异步操作，我们需要额外在特定生命周期中管理它们，以防止内存泄漏。
3. UI controller 比如 Activity 、Fragment 是设计用来渲染展示数据、响应用户行为、处理系统的某些交互。如果再要求他去负责加载网络或数据库数据，会让其显得臃肿和难以管理。所以为了简洁、清爽、丝滑，我们可以分离出数据操作的职责给 ViewModel。
4. Fragments 间共享数据。