---
layout: post
category: java
title: Java生成二维码分享海报
tagline: by 沉默王二
tag: java
---

这一篇文章我们就用 Java 来生成一下仿金山词霸的海报。


<!--more-->


>As long as you can still grab a breath, you fight.
只要一息尚存，就不得不战。

有那么一段时间，我特别迷恋金山词霸的每日一句分享海报。因为不仅海报上的图片美，文字也特别美，美得让我感觉生活都有了诗意。就像文章开头的那句中英文对照，中文和英文都妙极了。

最近，又有很多人迷恋上了流利说的小程序分享海报（朋友圈比比皆是）。但不管是金山词霸还是流利说，分享的海报都不是自己的二维码，这对于个人品牌的缔造者来说，实在是一件出力不讨好的事。

当然了，这种事难不倒作为程序员的我。

### 01、大致思路

- 采集网络图片

- 加载海报背景和个人品牌二维码

- 利用 Graphics2D 将网络图片绘制成海报封面

- 利用 Graphics2D 在海报上打印中英文对照语

- 利用 Graphics2D 在海报上绘制个人专属二维码

- 使用 Swing 构建图形化界面

- 将项目打成 jar 包发行

- 运行 jar 包，填写必要信息后生成海报


### 02、采集网络图片

第一步，获取网络图片的路径。金山词霸每日一句的图片路径地址形式如下所示。可以根据当前日期获取最新的图片路径。

```java
// 金山词霸的图片路径
String formatDate = DateFormatUtils.format(new Date(), "yyyyMMdd");
String picURL = "http://cdn.iciba.com/news/word/big_" + formatDate + "b.jpg";
```

第二步，有了图片路径后，可以根据此路径创建 HTTP get 请求。

```java
// 根据路径发起 HTTP get 请求
HttpGet httpget = new HttpGet(picURL);
// 使用 addHeader 方法添加请求头部
httpget.addHeader("Content-Type", "text/html;charset=UTF-8");

// 配置请求的超时设置
RequestConfig requestConfig = RequestConfig.custom().setConnectionRequestTimeout(500).setConnectTimeout(500)
		.setSocketTimeout(500).build();
httpget.setConfig(requestConfig);
```

第三步，创建 `CloseableHttpClient` 对象来执行 HTTP get 请求，并获取响应信息 `CloseableHttpResponse`。`CloseableHttpClient` 是一个抽象类，它是 `HttpClient` 的基本实现，也实现了 `java.io.Closeable` 接口。

```java
CloseableHttpClient httpclient = HttpClientBuilder.create().build();
CloseableHttpResponse response = httpclient.execute(httpget);
```

第四步，从 `CloseableHttpResponse` 中获取图片的输入流。

```java
HttpEntity entity = response.getEntity();
InputStream picStream = entity.getContent();
```

第五步，从图片输入流中读取信息，并输出到本地文件中。

```java
File pic = Files.createTempFile(Paths.get("D:\\test"), "pic_", ".jpg");
FileOutputStream fos = new FileOutputStream(pic);
int read = 0;

// 1024Byte(字节)=1KB 1024KB=1MB
byte[] bytes = new byte[1024 * 100];
while ((read = inputStream.read(bytes)) != -1) {
	fos.write(bytes, 0, read);
}

fos.flush();
fos.close();
```

在指定的临时目录下可以查看采集到的图片，如下所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-1.png)



### 03、加载海报背景和个人品牌二维码

海报背景的大小为 678 * 1013 像素，个人品牌二维码的大小为 128 * 128 像素。两张图片都是事先准备好的，放在 src 目录下。整个项目的目录结构图如下所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-2.png)

接下来，我们把这两张图片分别读取到临时文件当中，供后续动作使用。

第一步，创建 `ClassLoader` 对象，从 classpath 的根路径下查找资源。

```java
ClassLoader classLoader = ReadBgAndQrcode.class.getClassLoader();
```

第二步，通过 `classLoader.getResourceAsStream()` 读取海报背景和个人品牌二维码，复制到临时文件中。

```java
File bgFile = Files.createTempFile(DIRECTORY, "bg_", ".jpg").toFile();
InputStream inputStream = classLoader.getResourceAsStream("default_bgimg.jpg");
FileUtils.copyInputStreamToFile(inputStream, bgFile);
logger.debug("背景：" + bgFile.getAbsolutePath());


File qrcodeFile = Files.createTempFile(DIRECTORY, "qrcode_", ".jpg").toFile();
InputStream qrcodeInputStream = classLoader.getResourceAsStream("default_qrcodeimg.jpg");
FileUtils.copyInputStreamToFile(qrcodeInputStream, qrcodeFile);
logger.debug("二维码：" + qrcodeFile.getAbsolutePath());
```

在指定的临时目录下可以查看海报背景和个人品牌二维码，如下所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-3.png)

### 05、利用 Graphics2D 将网络图片绘制成海报封面

`Graphics2D` 类扩展了 `Graphics` 类，提供了对几何形状、坐标转换、颜色管理和文本布局更为复杂的控制，是用于呈现二维形状、文本和图像的基础类。

`BufferedImage` 使用可访问的图像数据缓冲区描述图像，由颜色模型和图像数据栅格组成，所有 BufferedImage 对象的左上角坐标为(0，0)。

可以利用 `BufferedImage` 类的 `createGraphics()` 方法获取 `Graphics2D` 对象。

第一步，将海报背景和海报封面读入到 BufferedImage 对象中。注意，`deleteOnExit()` 方法请求在虚拟机终止时删除此抽象路径名所表示的文件或目录。

```java
// 背景
File bgFile = FileUtil.read("bg_", ".jpg", "default_bgimg.jpg");
bgFile.deleteOnExit();
BufferedImage bgImage = ImageIO.read(bgFile);

// 封面图
File picFile = CapturePic.capture();
picFile.deleteOnExit();
BufferedImage picImage = ImageIO.read(picFile);
```

第二步，计算封面图的起始坐标，以及高度和宽度。

```java
// 封面图的起始坐标
int pic_x = MARGIN, pic_y = MARGIN;
// 封面图的宽度
int pic_width = bgImage.getWidth() - MARGIN * 2;
// 封面图的高度
int pic_height = picImage.getHeight() * pic_width / picImage.getWidth();
```

第三步，在海报背景上绘制封面图。

```java
Graphics2D graphics2d = bgImage.createGraphics();
// 在背景上绘制封面图
graphics2d.drawImage(picImage, pic_x, pic_y, pic_width, pic_height, null);
// 释放图形上下文，以及它正在使用的任何系统资源。
graphics2d.dispose();
```

第四步，将绘制好的图像输出到文件中。

```java
File posterFile = Files.createTempFile(FileUtil.DIRECTORY, "poster_", ".jpg").toFile();
ImageIO.write(bgImage, "jpg", posterFile);
```

在指定的临时目录下可以查看海报，如下所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-4.png)

### 06、利用 Graphics2D 在海报上打印中文

`Font` 类表示字体，用于以可见的方式呈现文本。字体提供了将字符序列映射到象形文字序列以及在图形和组件对象上呈现象形文字序列所需的信息。

第一步，通过 `GraphicsEnvironment` 类的 `getAvailableFontFamilyNames()` 查看计算机上允许使用的字体。

```java
String[] fontNames = GraphicsEnvironment.getLocalGraphicsEnvironment().getAvailableFontFamilyNames();

for (String fontName : fontNames) {
	System.out.println(fontName);
}
```

大致的中文字体有这么一些（还有更多，未列出）：

>宋体
幼圆
微软雅黑
微软雅黑 Light
新宋体
方正姚体
方正舒体
楷体
隶书
黑体

第二步，设置字体和颜色。

```java
// Font 的构造参数依次是字体名字，字体式样，字体大小
Font font = new Font("微软雅黑", Font.PLAIN, 28);
g.setFont(font);
// RGB
g.setColor(new Color(71, 71, 71));
```

第三步，根据当前字体下每个中文字符的宽度，以及海报可容纳的最大文本宽度，对文本进行换行。

计算每个字体的宽度时，需要用到 `sun.font.FontDesignMetrics`，它扩展了 `java.awt.FontMetrics`。`FentMetrics` 类定义了一个字体度量对象，该对象封装了有关在特定屏幕上呈现特定字体的信息。`FontDesignMetrics` 提供了更多指标的 Font 信息。

`FontDesignMetrics` 有几个重要的值需要说明一下：

- 基准点是 `baseline`
- ascent 是 baseline 之上至字符最高处的距离
- descent 是 baseline 之下至字符最低处的距离
- leading 文档说的很含糊，其实是上一行字符的 descent 到下一行的 ascent 之间的距离
- top 指的是指的是最高字符到 baseline 的值，即 ascent 的最大值
- bottom 指的是最下字符到 baseline 的值，即 descent 的最大值

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-5.png)

`FontDesignMetrics` 的 `charWidth()` 方法可以计算字符的宽度。

```java
public static String makeLineFeed(String zh, FontDesignMetrics metrics, int max_width) {
	StringBuilder sb = new StringBuilder();
	int line_width = 0;
	for (int i = 0; i < zh.length(); i++) {
		char c = zh.charAt(i);
		sb.append(c);

		// 如果主动换行则跳过
		if (sb.toString().endsWith("\n")) {
			line_width = 0;
			continue;
		}

		// FontDesignMetrics 的 charWidth() 方法可以计算字符的宽度
		int char_width = metrics.charWidth(c);
		line_width += char_width;

		// 如果当前字符的宽度加上之前字符串的已有宽度超出了海报的最大宽度，则换行
		if (line_width >= max_width - char_width) {
			line_width = 0;
			sb.append("\n");
		}
	}
	return sb.toString();
}
```

假如文本是“沉默王二，《Web 全栈开发进阶之路》作者；一个不止写代码的程序员，还写有趣有益的文字，给不喜欢严肃的你。”我们来通过 `makeLineFeed()` 方法试验一下。

```java
Font font = new Font("微软雅黑", Font.PLAIN, 28);
FontDesignMetrics metrics = FontDesignMetrics.getMetrics(font);

String zh = "沉默王二，《Web 全栈开发进阶之路》作者；一个不止写代码的程序员，还写有趣有益的文字，给不喜欢严肃的你。";

String[] rows = makeLineFeed(zh, metrics, 600).split("\n");
for (int i = 0; i < rows.length; i++) {
	System.out.println(rows[i]);
}
```

其结果如下所示。

>沉默王二，《Web 全栈开发进阶之路》作者；
一个不止写代码的程序员，还写有趣有益的文字
，给不喜欢严肃的你。

第四步，将自动换行后的文本在海报背景上打印。

这里需要用到 `FontDesignMetrics` 的 `getHeight()` 方法获取每行文本的高度。对照下面的示意图，理解 height 的具体高度。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-6.png)


```java
// 自动换行后的文本
String zhWrap = FontUtil.makeLineFeed(graphics2dPoster.getZh(), metrics, graphics2dPoster.getSuitableWidth());

// 拆分行
String[] zhWraps = zhWrap.split("\n");

// 将每一行在海报背景上打印
for (int i = 0; i < zhWraps.length; i++) {
	graphics2dPoster.addCurrentY(metrics.getHeight());
	graphics2d.drawString(zhWraps[i], MARGIN, graphics2dPoster.getCurrentY());
}
```

此时的海报效果如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-7.png)

可以看得出，文字带有很强的锯齿感，怎么消除呢？

```java
graphics2d.setRenderingHint(RenderingHints.KEY_TEXT_ANTIALIASING, RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
```

如果英语不好的话，看起来这段代码会很吃力。`ANTIALIASING` 单词的意思就是“消除混叠现象，消除走样，图形保真”。

### 07、利用 Graphics2D 在海报上打印英文

英文和中文最大的不同在于，换行的单位不再是单个字符，而是整个单词。

第一步，根据当前字体下每个英文单词的宽度，以及海报可容纳的最大文本宽度，对文本进行换行。

```java
public static String makeEnLineFeed(String en, FontDesignMetrics metrics, int max_width) {
	// 每个单词后追加空格
	char space = ' ';
	int spaceWidth = metrics.charWidth(space);

	// 按照空格对英文文本进行拆分
	String[] words = en.split(String.valueOf(space));
	// 利用 StringBuilder 对字符串进行修改
	StringBuilder sb = new StringBuilder();
	// 每行文本的宽度
	int len = 0;

	for (int i = 0; i < words.length; i++) {
		String word = words[i];

		int wordWidth = metrics.stringWidth(word);
		// 叠加当前单词的宽度
		len += wordWidth;

		// 超出最大宽度，进行换行
		if (len > max_width) {
			sb.append("\n");
			sb.append(word);
			sb.append(space);

			// 下一行的起始宽度
			len = wordWidth + spaceWidth;
		} else {
			sb.append(word);
			sb.append(space);

			// 多了一个空格
			len += spaceWidth;
		}
	}
	return sb.toString();
}
```

假如文本是“Fear can hold you prisoner. Hope can set you free. It takes a strong man to save himself, and a great man to save another.”我们来通过 makeEnLineFeed() 方法试验一下。

```java
Font font = new Font("微软雅黑", Font.PLAIN, 28);
FontDesignMetrics metrics = FontDesignMetrics.getMetrics(font);

String en = "Fear can hold you prisoner. Hope can set you free. It takes a strong man to save himself, and a great man to save another.";
String[] rows = makeEnLineFeed(en, metrics, 600).split("\n");
for (int i = 0; i < rows.length; i++) {
	System.out.println(rows[i]);
}
```

其结果如下所示。

>Fear can hold you prisoner. Hope can set 
you free. It takes a strong man to save 
himself, and a great man to save another. 

第三步，将自动换行后的文本在海报背景上打印。

```java
// 设置封面图和下方中文之间的距离
graphics2dPoster.addCurrentY(20);

Graphics2D graphics2d = graphics2dPoster.getGraphics2d();
graphics2d.setColor(new Color(157, 157, 157));

FontDesignMetrics metrics = FontDesignMetrics.getMetrics(graphics2d.getFont());
String enWrap = FontUtil.makeEnLineFeed(graphics2dPoster.getEn(), metrics, graphics2dPoster.getSuitableWidth());
String[] enWraps = enWrap.split("\n");
for (int i = 0; i < enWraps.length; i++) {
	graphics2dPoster.addCurrentY(metrics.getHeight());
	graphics2d.drawString(enWraps[i], MARGIN, graphics2dPoster.getCurrentY());
}
```

此时的海报效果如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-8.png)

### 07、利用 Graphics2D 在海报上绘制个人专属二维码

有了前面绘制海报封面的经验，绘制二维码就变得轻而易举了。

```java
// 二维码
File qrcodeFile = FileUtil.read("qrcode_", ".jpg", "default_qrcodeimg.jpg");
qrcodeFile.deleteOnExit();

BufferedImage qrcodeImage = ImageIO.read(qrcodeFile);
// 二维码起始坐标
int qrcode_x = bgImage.getWidth() - qrcodeImage.getWidth() - MARGIN;
int qrcode_y = bgImage.getHeight() - qrcodeImage.getHeight() - MARGIN;
graphics2dPoster.getGraphics2d().drawImage(qrcodeImage, qrcode_x, qrcode_y, qrcodeImage.getWidth(),
		qrcodeImage.getHeight(), null);
```

此时的海报效果如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-9.png)

是不是感觉海报的左下角比较空白，整体的对称性不够自然，那就在左下角追加一些二维码的描述文本吧。

```java
graphics2d.setColor(new Color(71, 71, 71));
Font font = new Font(USE_FONT_NAME, Font.PLAIN, 22);
graphics2d.setFont(font);
FontDesignMetrics metrics = FontDesignMetrics.getMetrics(graphics2d.getFont());

graphics2d.drawString("沉默王二", MARGIN, bgImage.getHeight() - MARGIN - metrics.getHeight() * 2);
graphics2d.drawString("一个幽默的程序员", MARGIN, bgImage.getHeight() - MARGIN - metrics.getDescent());
```

此时的海报效果如下图所示。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-10.png)


### 08、使用 Swing 构建图形化界面

Swing 是一个用于 Java GUI 编程（图形界面设计）的工具包（类库）；换句话说，Java 之所以可以用来开发带界面的 PC 软件，就是因为 Swing 的存在。

Swing 使用纯粹的 Java 代码来模拟各种控件，没有使用本地操作系统的内在方法，所以 Swing 是跨平台的。也正是因为 Swing 的这种特性，人们通常把 Swing 控件称为轻量级控件。

Eclipse 默认是不支持可视化的 Swing 编程的，但 Eclipse 的插件市场上有这样一个好插件——WindowBuilder，使用它可以大幅度地降低开发难度，迅速地提升开发效率。

下载地址：https://marketplace.eclipse.org/content/windowbuilder

可直接拖拽到 Eclipse 进行安装，如下图。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-11.png)

注意，Eclipse 的版本要求为：

>2018-09 (4.9), Photon (4.8), Oxygen (4.7), Neon (4.6), 2018-12 (4.10), 2019-03 (4.11)

拖拽到 Eclipse 后的效果如下：

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-12.png)


安装完成后，会提醒你重启 Eclipse。

温馨提示：安装的过程大约持续 3 分钟的时间，中间可能会失败，重试几次即可。不用担心，Eclipse 会智能地保存失败前的进度。

安装成功后，就可以使用可视化工具设计界面了，如下图所示：

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-13.png)

### 09、将项目打成 jar 包发行

在将应用程序进行打包时，使用者都希望开发者只提供一个单独的文件，而不是包含大量源码的文件夹。jar 包存在的目的正源于此。

将项目打成 jar 包也很简单，在 Eclipse 中，可依次右键项目→Export→Runnable JAR file。你将会看到以下界面。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-14.png)

选择 main 方法所在类，指定导出目标，选择 `Copy required libraries` 选项，点击「Finish」即可。在指定的目录下可找到生成的 jar 包文件。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-15.png)

### 10、运行 jar 包，填写必要信息后生成海报

如果电脑上安装了 Java 的运行环境，双击该 jar 包文件就可以运行。运行后的界面，如下图所示。可以填写中文、英文、海报封面路径，然后点击按钮生成海报。

![](http://www.itwanger.com/assets/images/2019/11/java-qrcode-poster-16.png)

PS：为了便于大家的学习，我已经将源码放在了 GitHub 上，地址如下。

https://github.com/qinggee/poster/tree/jinshanciba

赶快去 star 吧！





上一篇：[Java ：接口和抽象类，傻傻分不清](http://www.itwanger.com/java/2019/11/14/java-extends.html)

下一篇：[Java：优雅地处理异常真是一门学问啊！](http://www.itwanger.com/java/2019/11/14/java-exception.html)



谢谢大家的阅读，原创不易，喜欢就随手点个赞👍，这将是我最强的写作动力。如果觉得文章对你有点帮助，还挺有趣，就关注一下我的公众号「**沉默王二**」。
