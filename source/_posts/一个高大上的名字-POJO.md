---
title: 一个高大上的名字-POJO
date: 2019-06-05 10:37:55
tags: [databinding,java]
categories: Jetpack
---

##### POJO

名字乍一看都会被吓到，但是POJO其实很简单，下面的段落部分摘抄自 [这位博主](https://www.jianshu.com/p/6f3e2bd50cb1) 。

POJO的创始人([martinfowler](https://link.jianshu.com/?t=https://www.martinfowler.com/bliki/POJO.html))博客：

The term was coined while Rebecca Parsons, Josh MacKenzie and I were preparing for a talk at a conference in September 2000. In the talk we were pointing out the many benefits of encoding business logic into regular java objects rather than using Entity Beans. We wondered why people were so against using regular objects in their systems and concluded that it was because simple objects lacked a fancy name. So we gave them one, and it's caught on very nicely.
...在谈话中我们指出，编写业务逻辑的时候，使用常规的java对象要比实体bean要好的多。我们怀疑为什么一些人极力反对在他们的代码中使用常规对象，还辩解称因为这些常规对象没有一个花哨的名字，所以我们给他们起了一个非常好听的名字。（Plain Old Java Object）

维基百科原文

The term "POJO" initially denoted a Java object which does not follow any of the major Java object models, conventions, or frameworks; nowadays "POJO" may be used as an acronym for "Plain Old JavaScript Object" as well, in which case the term denotes a JavaScript object of similar pedigree.[2]
术语POJO起初表示为不遵任何主要的java模型，约定或者框架的java对象，现在，pojo也可以用作'Plain Old JavaScript Object'的缩写，这样的话和javascript对象有着相似的渊源。

##### JavaBean

[维基百科](https://link.jianshu.com/?t=https://en.wikipedia.org/wiki/JavaBeans)中JavaBeans的概念

A JavaBean is a POJO that is serializable, has a no-argument constructor, and allows access to properties using getter and setter methods that follow a simple naming convention. Because of this convention, simple declarative references can be made to the properties of arbitrary JavaBeans. Code using such a declarative reference does not have to know anything about the type of the bean, and the bean can be used with many frameworks without these frameworks having to know the exact type of the bean. The JavaBeans specification, if fully implemented, slightly breaks the POJO model as the class must implement the Serializable interface to be a true JavaBean. Many POJO classes still called JavaBeans do not meet this requirement. Since Serializable is a marker (method-less) interface, this is not much of a burden.
JavaBean是一个可序列化的POJO，具有一个无参构造器，并且允许使用遵循简单命名约定的getter和setter方法来访问属性。由于这个惯例，可以对任意JavaBean属性进行简单的声明引用。使用这种声明引用的代码不需要知道bean的具体类型。并且，这个bean还可以被很多框架使用，这些java框架也不需要知道bean的类型。由于java.io.Serializable是一个标记接口（无方法），所以这并不是一个多大的负担。如果JavaBean完全实现的话，稍微打破了一些POJO模型。很多被称之为JavaBean的POJO类并不符合这个要求，因为JavaBean必须实现Serializable接口才能成为真正的JavaBean。

有些人看上面一堆肯定头疼，不如上点代码：

```java
public class POJO {
    private String name;
    private String id;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
}
```

```java
 public class JavaBean implements Serializable{
    private String name;
    private String id;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
    public void eat(){
        System.out.println(name+"吃了满汉全席！");
    }
}
```

##### 总结来讲：

- POJO就是一个只有属性和get、set方法的简单类。
- JavaBean在POJO的基础上实现了持久化并且使其拥有了行为。