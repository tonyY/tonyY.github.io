---
layout: post
category: java
title: JDK 和 JRE 有什么区别
tagline: by 沉默王二
tag: [java,重学Java]
---

[上一篇](http://www.itwanger.com/java/2019/10/19/java-jdk.html)，我们谈了什么是 JDK。这一篇我们来谈谈 JDK 和 JRE 之间的区别，因为很多初学者搞混它们。
<!--more-->

JRE 是 `Java Runtime Environment` 的缩写，是 Java 程序的运行环境。既然是运行环境，自然包含 JVM（Java 虚拟机），还有所有 Java 类库的 class 文件。

JDK 的 bin 目录比 JRE 的 bin 目录多了一个 javac，这一点很好理解，因为 JRE 只是一个运行环境而已，与开发无关，与编译无关。

换个通俗点的说法就是，JDK 是 Java 的开发包，包含有 jre，面向的是开发人员；jre 仅仅是 Java 的运行时环境，面向的是 Java 程序的使用者；而 JDK 包含了同版本的 JRE，此外还包含有编译器和其它工具。




