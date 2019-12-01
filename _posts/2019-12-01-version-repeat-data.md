---
layout: post
category: java
title: 行数据上加一个version版本字段，可以有效防止数据重复更新
tagline: by 沉默王二
tag: java
---

有时候，可能因为编码上的一些漏洞被利用，导致代码部分的check失效，一些重复请求会同时更新一条数据，导致出现问题。在行数据上加一个 version 版本字段，可以为程序加上最后一道屏障。

<!--more-->

原理是什么呢？

第一，更新数据之前先获取行数据的版本 version；
第二，重复请求第一次，更新行数据，version+1；
第三，重复请求第二次，判断 version ≠ version，报错。

具体的做法。

第一步，增加 version 版本字段。

![](http://www.itwanger.com/assets/images/2019/12/version-repeat-data-1.png)

第二步，更新数据之前获取版本。

```
long version = select version from test where id = 1;
```

*PS：代码是模拟的*。

第三步，更新数据的时候（Mybatis）增加 where 条件 `version = #{version}`，并且更新 version 字段。

```
update test money=money+#{money},version=version+1 where id = 1 and version = #{version}
```

整体的思路非常简单，但是对数据的安全性保护却非常有效。如果没有 version 字段保护，那么当重复请求逃过代码的检查时，数据库就会更新出错。但是有了 version 字段保护后，重复数据就被拒之门外了。

防止重复数据的第一步：[jQuery 禁用表单提交按钮，防止用户请求重复提交](http://www.itwanger.com/web/2019/11/29/web-jquery-form-disable-submit.html)

------

好了各位读者朋友们，以上就是本文的全部内容了。**能看到这里的都是人才，二哥必须要为你点个赞**👍。如果觉得不过瘾，还想看到更多，可以查看我的[个人博客](http://www.itwanger.com/)。如果你有什么问题需要我的帮助，或者想喷我了，欢迎留言哟。

养成好习惯！如果觉得这篇文章有点用的话，**求点赞、求关注、求分享、求留言**，这将是我写下去的最强动力！



