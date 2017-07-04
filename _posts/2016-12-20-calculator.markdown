---
layout:     post
title:      "智能计算器"
subtitle:   "一款实用性超强的科学计算器"
date:       2016-12-20 12:00:00
author:     "SnailStudio"
header-img: "img/post-bg-2015.jpg"
tags:
    - Android
---

> “Yeah It's on. ”


## 前言

这是一款非常强大的计算器。它有以下优势：

* 支持复杂的四则混合运算。
* 支持自定义常量和函数的计算，并支持常量和函数的嵌套使用。
* 支持矩阵的计算。

同时这也是我的第一个Kotlin项目。

<p id = "code"></p>

---

## 项目源码

[https://github.com/xuqiqiang/Calculator](https://github.com/xuqiqiang/Calculator)

---

## 项目架构

该App完全使用了Kotlin语言开发，遵循Android-CleanArchitecture思想，基于MVP模式，并使用如下主流开源框架：

* Kotlin
* Anko
* Dagger2
* Rxjava
* DataBinding

---

## Kotlin

本人已经将Kotlin运用在项目开发2个月了，期间难免有一些坑，但是在坑之外，是他带给我的快感，从此以后。用Kotlin写代码相较于Java完全是一种享受。

Kotlin是又一个基于JVM的语言，由JetBrains开发(你用的Android Studio就是他家的)。如果你有java基础，那么他上手极其容易。

除了无缝调用java(所有java类、java库皆可不作任何处理的兼容)、一键将java转为Kotlin、空指针安全这些特性，还有许多比Java屌的特性。下面举一些栗子：

#### 代码量对比

`Java`

```java
TextView textView = findViewById(R.id.textView);

textView.setText("Hello World");
```

`Kotlin`

```java
textView.text = "hello kotlin"
```

扩展函数简单来说，就是将某个类不通过继承动态扩展，来添加方法等，比如下面的toast就是扩展了Context类。

`Java`

```java
Button button = findviewbyid(R.id.button)
button.setOnClickListener(new View.OnClickListener() {
@Override public void onClick(View v) {
Toast.makeText(this,"hello java",Toast.LENGTH_SHORT);
}
});
```

`Kotlin`

```java
button.setOnClickListener {toast("hello kotlin")}
```

#### POJO类(Java Bean对比)

`Java`

```java
public class User {
private String name;
private String id;

public User(String name, String id) {
this.name = name;
this.id = id;
}

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

`kotlin`(不要被吓到，确实这么短！！)

```java
data class User(var name: String, var id: String)
```

#### 像写文章一样使用 Kotlin

不知道大家有没有看到过下面的函数调用：

```c
print "Hello World"
```

这样的感觉就好像在写文章，没有括号，让语言的表现力进一步增强。Groovy 和 Scala 就有这样的特性，那么 Kotlin 呢？

```java
for(i in 0..10){
    ...
}
```

如果你在 Kotlin 中写下了这样的代码，你可能觉得很自然，不过你有没有想过 0..10 究竟是个什么？

“是个 IntRange 啊，这你都不知道。”你一脸嫌弃的回答道。

是啊，确实是个 IntRange，不过为什么是 0..10 返回个 IntRange，而不是 0->10 呢？

“我靠。。这是出厂设定，懂不懂。。”你开始变得更嫌弃了。。

额，其实我想说的是，你知不知道这其实是个运算符重载？！

```java
public final operator 
    fun rangeTo(other: kotlin.Int)
    : kotlin.ranges.IntRange { 
    ...
}
```

没错，Kotlin 允许我们对运算符进行重载，所以你甚至可以给 String 定义 rangeTo 方法。

“毕竟运算符是有限的吧？如果说我想给 Person 增加个 say 方法，不带括号那种，怎么办？” 你不以为然地说。

这个嘛。。当然也是可以哒！

```java
class Person(val name: String){
    infix fun 说(word: String){
        println("\"$name 说 $word\"")
    }
}

fun main(args: Array<String>) {
    val 老张 = Person("老张")
    老张 说 "小伙砸,你咋又调皮捏!"
}
```

这段代码执行结果是：

```xml
老张 说 "小伙砸,你咋又调皮捏!"
```

栗子完毕。相信看到上面的一些代码，大家心里已经比较清楚kotlin的特点了。相比java，不仅功能更强大，代码少了至少三倍。这简直是大快人心！

---

## MVP架构

#### 简介

通过契约类Contract管理View Model Presenter接口(如果你项目写烦了MVP，那么安利下自动生成MVP代码的插件[MVPHelper](https://github.com/githubwing/MVPHelper)。

* Model — 主要处理业务，用于数据的获取(如网络、本地缓存)。
* View — 用于把数据展示，并且提供交互。
* Presenter — View和Model交互的桥梁，二者通过Presenter建立联系。

主要流程如下：

用户与View交互，View得知用户需要加载数据，告知Presenter，Presenter则告知Model，Model拿到数据反交于Prsenter，Presenter将数据交给View进行展示。

偷一张老图：

![MVP框架图](/img/calculator/p1.png)

#### CleanArchitecture

为什么倾向于CleanArchitecture，那一定是有他的道理的。对比传统开发的MVC开发方式，你会得到以下好处：

* 代码复用性更高
* 更易于测试
* 耦合度更小

关于探讨CleanArchitecture架构方面的文章很多，但是究其源头无非都是出自uncle-bob 叔叔的这篇[《The Clean Architecture》](#documents)。

下面这幅图，是googlesample下面的了。

![googlesample](/img/calculator/p3.png)

下面这幅图，是uncle-bob画的了。

![CleanArchitecture](/img/calculator/p5.png)

细心的你已经发现了，这两个图其实是一个意思。从大的方向上看，都是三层结构。

> DataLayer

最底层，完全不知道有`DomainLayer`，`PresentationLayer`的存在，听到这里，你还在怀疑这个架构的`可测试性`和`耦合度低`吗？那么`DataLayer`的主要职责是什么？
1、从网络获取数据，向网络提交数据，总之就是和网络打交道。
2、从本地DB，shareprefence等等，内存等，总之就是本地获取数据，缓存数据，总之就是和本地数据打交道的。
这也就是你为什么看到很多Android-CleanArchitecture的package里面有一个local，和一个remote了，然而是否有必要分的这么细，个人习惯啊~，不强求。反正这一层如果出现了anroid.os*,我就更你拼了，对不起，你已经偏离了Android-CleanArchitecture了。

> DomainLayer

中间层，他完全不知道有一个`PresentationLayer`存在，他只知道，有`DataLayer`，他可以基于这些数据，建立很多玩法，比如去网络拿一堆名人回来，然后将这些数据缓存到本地，在比如，他写了一篇黑某明星的文章，将文字发布到网上等等。因此他的主要职责是:
1、控制`DataLayer`对数据做**增删改查**，没错，就这么简单，然后就没有然后了。
2、真的没有了，不骗你，但是这一层如果出现了anroid.os*,我就更你拼了，对不起，你已经偏离了Android-CleanArchitecture了。

> PresentationLayer

最上层，他知道`DomainLayer`，有人要问了，那么他知道`DataLayer`，回答，他知道你妹~ 他累不累啊，要知道这么多？
因此，它只知道`DomainLayer`，那么他的职责有哪些？
1、通知`DomainLayer`有活干了，根据`DomainLayer`反馈变化界面
2、通知`DomainLayer`有活干了，根据`DomainLayer`反馈变化界面
3、通知`DomainLayer`有活干了，根据`DomainLayer`反馈变化界面
这年头，重要的时间一定要说三遍，而且，就是这么任性~~

分析了每层之后，我们发现，依赖的关系是 **PresentationLayer** --> **DomainLayer** --> **DataLayer** 的。
**DomainLayer** --> **DataLayer** 不知道有android平台的存在。
因此，只要我们围绕这个原则去做架构，那么就称的上是Android-CleanArchitecture。

---

## Dagger2

项目中,主要进行presenter、model、view等类的注入操作。这里由于篇幅过长，本文不再详细做Dagger2用法解释。Dagger2在Kotlin中使用有一些配置是不一样的，详细配置请看[项目源码](#code)。

**ApplicationComponent**

主Component、用于注入AppComponent、便于提供子Component依赖。

依赖于: ApplicationModule(提供Context、ThreadExecutor、CalculatorRepository、Speaker等)

**ActivityComponent**

父Component为ApplicationComponent，用于注入BaseBindingActivity

依赖于: ActivityModule(提供Activity)

**CalculatorComponent**

父Component为ApplicationComponent，用于注入CalculatorPresenter等

依赖于 : CalculatorModule

---

## 参考文档

<p id = "documents"></p>
[《RFC3550》](https://tools.ietf.org/html/rfc3550)<br/>
[《The Clean Architecture》](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)

—— SnailStudio 后记于 2017.5
