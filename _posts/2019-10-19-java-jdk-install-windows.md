---
layout: post
category: java
title: Windows 安装 JDK 与环境变量配置
tagline: by 沉默王二
tag: [java,重学Java]
---

Windows 安装 JDK 与环境变量配置，超级详细。
<!--more-->

### 一、准备工具

1. JDK  

JDK 1.8 的官网下载地址：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

2. 系统

查看自己的 Windows 系统，是 64 位还是 32 位。

![](http://www.itmind.net/wp-content/uploads/2019/10/763a7b405375ad743d8df089fd61618d.png)


选择对应的 JDK。

![](http://www.itmind.net/wp-content/uploads/2019/10/4f374ae1e89a3a9044bdfb6fb574c729.png)


### 二、安装步骤

1. 安装 JDK 和 JRE， 选择安装目录。

[什么是 JDK](http://www.itwanger.com/java/2019/10/19/java-jdk.html)

[JDK 和 JRE 有什么区别](http://www.itwanger.com/java/2019/10/19/java-jdk-jre.html)



安装过程中会出现两次安装提示 。第一次是安装 JDK ，第二次是安装 JRE。

1）双击 jdk-8u25-windows-x64.exe 进行安装。
![](http://www.itmind.net/wp-content/uploads/2019/10/32c1e793c3877bbc105897ef347995d8.png)

2）点击“下一步”继续。

![](http://www.itmind.net/wp-content/uploads/2019/10/2eb460524fe5013ecbfc0987e2511c28.png)

3）选择安装路径，然后点击下一步。

![](http://www.itmind.net/wp-content/uploads/2019/10/cb11195ce78ec15a454c322ed41c4503.png)

4）等待安装结束之后。选择 JRE 安装的路径，点击下一步。


![](http://www.itmind.net/wp-content/uploads/2019/10/d186ba95e67f02ad45f249087cc68d37.png)



5）JRE 的安装。

![](http://www.itmind.net/wp-content/uploads/2019/10/2b249a4587d0ba6ed565e0905869388a.png)

6）安装中。


7）安装完成，点击关闭。

![](http://www.itmind.net/wp-content/uploads/2019/10/18ff88c0c34809f18ea569d2ab13c2df.png)

### 三、配置环境变量

配置环境变量的入口：右击“我的电脑”-->"高级"-->"环境变量"。

1）JAVA_HOME 环境变量。

它指向 JDK 的安装目录，Eclipse（我最爱的集成开发工具）等软件就是通过搜索 JAVA_HOME 变量来找到并使用安装好的 JDK 的。

配置方法：在系统变量里点击新建，变量名填写 JAVA_HOME，变量值填写 JDK 的安装路径。（根据自己的安装路径填写）

JAVA_HOME：D:\Java\jdk1.8.0_25

![](http://www.itmind.net/wp-content/uploads/2019/10/cdd0d1f65727c0c3cba1942b47ec6b03.png)


2）PATH 环境变量

指定命令的索引路径，在命令行下面执行命令如 javac 编译 Java 程序时，它会到 PATH 变量所指定的路径中查找相应的命令程序。

我们需要把 JDK 的 bin 目录增加到现有的 PATH 变量中，bin 目录中包含了经常要用到的可执行文件如 javac/java/javadoc/javap 等，设置好 PATH 变量后，就可以在任何目录下执行之前提到的命令了。

在系统变量里找到 PATH 变量，这是系统自带的，不用新建。双击 PATH，由于原来的变量值已经存在，所以在已有的变量后添加上`;%JAVA_HOME%\bin;` 即可，注意前后的分号。

![](http://www.itmind.net/wp-content/uploads/2019/10/2c0ca0b16e8df37fa683ad739e8c629c.png)

然后点击确定完成。

### 四、 测试环境

运行 cmd，分别输入 `javac -verson` 和 `java -version`。

![](http://www.itmind.net/wp-content/uploads/2019/10/466eaab55ad59d2a52b2f57ded356468.png)

出现以上结果则表明安装成功。

