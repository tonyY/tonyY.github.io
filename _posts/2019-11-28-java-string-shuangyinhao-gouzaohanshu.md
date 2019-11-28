---
layout: post
category: java
title: 灵魂拷问：创建 Java 字符串，用""还是构造函数
tagline: by 沉默王二
tag: java
---

在逛 programcreek 的时候，我发现了一些小而精悍的主题。比如说：创建 Java 字符串，用 "" 还是构造函数？像这类灵魂拷问的主题，非常值得深入地研究一下。


<!--more-->

### 01、""

来看这样一串代码：

```java
String a = "沉默王二";
String b = "沉默王二";
System.out.println(a == b);  // true
System.out.println(a.equals(b)); // true
```

`a==b` 是因为 a 和 b 指向的是方法区中同一个字符串常量值。当相同的字符串被创建多次时，只会保存字符串常量的一份副本。画个图表示一下。

![](http://www.itwanger.com/assets/images/2019/11/java-string-shuangyinhao-gouzaohanshu-1.png)




### 02、构造函数

来看这样一串代码：

```java
String a = new String("沉默王二");
String b = new String("沉默王二");
System.out.println(a == b);  // false
System.out.println(a.equals(b)); // true
```

`a≠b` 是因为 a 和 b 指向的是堆中不同的字符串对象，不同的对象，它们的对象引用也是不同的。画个图表示一下。

![](http://www.itwanger.com/assets/images/2019/11/java-string-shuangyinhao-gouzaohanshu-2.png)

### 03、总结

字符串“沉默王二”本身已经是一个字符串类型，再通过 new 关键字通过构造函数创建字符串对象就显得多此一举。所以，如果你只需要一个字符串对象，使用双引号——"" 即可。除非你想在堆中创建一个新的字符串对象。

最后，谢谢大家的阅读。后续还会继续更新《灵魂拷问》系列，我想带着大家在“知其所以然”方面多下下功夫。

