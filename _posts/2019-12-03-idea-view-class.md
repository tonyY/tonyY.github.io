---
layout: post
category: java
title: IDEA 如何查看 class 文件反编译后的内容
tagline: by 沉默王二
tag: java
---

有时候，我们需要查看 IDEA 编译后的 class 文件是什么样子的，字节码不太能看得懂，就需要再进行反编译。怎么做呢？


<!--more-->

项目的目录下有一个 target，根据报名找到对应的 class 文件双击打开即可。

![](http://www.itwanger.com/assets/images/2019/12/idea-view-class-1.png)


IDEA 默认会使用 Fernflower 对字节码文件进行反编译。反编译后的内容如下所示。

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.cmower.java_demo.stackoverflow;

public class Cmower1 {
    public Cmower1() {
    }

    public static void main(String[] args) {
        String[] names = new String[]{"沉", "默", "王", "二"};
        String[] var2 = names;
        int var3 = names.length;

        for(int var4 = 0; var4 < var3; ++var4) {
            String name = var2[var4];
            System.out.println(name);
        }

    }
}
```

源文件长什么样子呢？

```java
package com.cmower.java_demo.stackoverflow;

public class Cmower1 {
public static void main(String[] args) {
String[] names = { "沉", "默", "王", "二" };

    for (String name : names) {
        System.out.println(name);
    }

}
}
```

可以看得出，javac 会帮助我们对源文件进行一些编译优化。比如说：

1）{}声明的数组最终还是用的 new 关键字。
2）增强的 for 循环最终还是变成了普通的 for 循环语句。


系列文章：

[如何安装 IntelliJ IDEA 最新版本——详细教程](http://www.itwanger.com/java/2019/11/25/java-idea-community.html)

[IDEA 如何自动导入（import）](http://www.itwanger.com/java/2019/11/26/java-idea-import-auto.html)


