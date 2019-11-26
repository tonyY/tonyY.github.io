---
layout: post
category: java
title: IDEA 如何自动导入（import）
tagline: by 沉默王二
tag: java
---

如果大家正在使用一个未曾导入（import）过的类，或者它的静态方法或者静态字段，IDEA 会给出对应的建议，只要按下 ⌥（option）和回车就可以接受建议。


<!--more-->


![](http://www.itwanger.com/assets/images/2019/11/java-idea-import-auto-1.png)

但我觉得这样做仍然很麻烦，不够智能化。怎么办呢？

打开 IDEA 的首选项，找到 Editor | General | Auto Import。勾选上 `Add unambiguous imports on the fly` 和 `Optimize imports on the fly (for current project)`。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-import-auto-2.png)

点击「OK」后，导入语句就会在复制代码的时候自动导入了。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-import-auto-3.png)

当然了，前提条件是导入语句是单一确定的，如果有多个选项，IDEA 仍然会给出对应的建议供你手动挑选。


好了，大功告成！给自己鼓个掌吧。