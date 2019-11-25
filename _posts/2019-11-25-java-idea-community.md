---
layout: post
category: java
title: 如何安装 IntelliJ IDEA 最新版本——详细教程
tagline: by 沉默王二
tag: java
---

IntelliJ IDEA 简称 IDEA，被业界公认为最好的 Java 集成开发工具，尤其在智能代码助手、代码自动提示、代码重构、代码版本管理(Git、SVN、Maven)、单元测试、代码分析等方面有着亮眼的发挥。IDEA 产于捷克，开发人员以严谨著称的东欧程序员为主。IDEA 分为社区版和付费版两个版本。


<!--more-->




我呢，一直是 [Eclipse](http://www.itwanger.com/java/2019/10/22/eclipse-install.html) 的忠实粉丝，差不多十年的老用户了。很早就接触到了 IDEA，但一直用不习惯 IDEA，不过为了与时俱进，最近开始下定决心——硬面刚。

### 01、下载 IDEA

[我](https://mp.weixin.qq.com/s/feoOINGSyivBO8Z1gaQVOA)建议大家从官网上下载软件，免得被某些软件园捆绑恶意插件，烦不胜烦。IntelliJ IDEA 的官方下载地址为：[https://www.jetbrains.com/idea/download/](https://www.jetbrains.com/idea/download)

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-1.png)


UItimate 为付费版，可以免费试用，主要针对的是 Web 和企业开发用户；Community 为免费版，可以免费使用，主要针对的是 Java 初学者和安卓开发用户。

功能上的差别如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-2.png)

本篇教程主要针对的是 Java 初学者，所以选择免费版为例，点击「Download」进行下载。专业级程序员可以参考：[IntelliJ IDEA 付费版安装和激活教程](http://www.itwanger.com/java/2019/10/22/idea-windows-install.html)。

稍等一分钟时间，大概 580M。

### 02、安装 IDEA

双击运行 IDEA 安装程序，一步步傻瓜式的下一步就行了。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-3.png)


![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-4.png)


![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-5.png)

为了方便启动 IDEA，可以勾选【64-bit launcher】复选框。为了关联 Java 源文件，可以勾选【.java】复选框。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-6.png)

点击【Install】后，需要静静地等待一会，大概一分钟的时间，趁机休息一下眼睛。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-7.png)

安装完成后的界面如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-8.png)

### 03、启动 IDEA

回到桌面，双击运行 IDEA 的快捷方式，启动 IDEA。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-9.png)

假装阅读完条款后，勾选同意复选框，点击【Continue】

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-10.png)

如果想要帮助 IDEA 收集改进信息，可以点击【Send Usage Statistics】；否则点击【Don't send】。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-11.png)

点击【Create New Project】，创建一个新的项目。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-12.png)

我电脑上默认安装的是 [JDK](http://www.itwanger.com/java/2019/10/19/java-jdk.html) 1.8。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-13.png)

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-14.png)

给项目起一个英文名字，点击【Finish】。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-15.png)

启动成功后的界面如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-16.png)

点开 cmower 节点，可以查看项目创建成功后的目录结构图。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-16.png)

1）.idea 目录里有一些 xml 文件，包含了项目的历史记录和版本控制信息。

2）src 为 Java 源文件的根目录。

3）cmower.iml 文件为当前项目的配置信息。

4）External Libraries 里包含了项目依赖的 jar 包。

### 04、使用 IDEA 创建 hello world 程序

既然 IDEA 已经安装成功了，不妨来一发 hello world 程序。

右键 src 目录，依次选择 New→Java Class

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-17.png)

输入类名后，选择 Class 选项。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-18.png)

IDEA 创建的第一个 Java 类就完成了。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-19.png)

在大括号中敲 psvm，然后再敲回车就可以创建 [main 方法](http://www.itwanger.com/java/2019/11/01/java-mian-class.html)。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-20.png)

其中 psvm 为 `public static void main` 的首字母缩写。整个 hello world 程序的代码如下所示。

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("沉默王二，一枚有趣的程序员");
    }
}
```

整个过程不需要按住 `Ctrl + S` 进行保存，IDEA 会自动帮我们保存——再也不用担心源码丢失了，IDEA 真贴心。

在当前代码编辑窗口下右键选择【Run 'Test.main()'】运行当前程序。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-21.png)

执行结束后会在控制台输出以下信息。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-22.png)

如果更改了源码，再次运行时可以直接点击下图中绿色的“运行”按钮即可，不需要再右键选择【Run 'Test.main()'】了。

![](http://www.itwanger.com/assets/images/2019/11/java-idea-community-23.png)


好了，大功告成！给自己鼓个掌吧。如果你觉得文章写得非常用心，可以关注一下我的微信公众号【沉默王二】，回复【1024】有惊喜哦。