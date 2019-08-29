---
layout: post
category: 其他
title: gittalk Error: Validation Failed.
tagline: by 沉默王二
tags: 
  - 其他
---

在配置 gittalk 的时候，可能出现 `gittalk Error: Validation Failed.`，相关的问题 issue，参照链接：https://github.com/gitalk/gitalk/issues/102

<!--more-->
但假如你觉得很麻烦的话，我也可以直接告诉你答案。引发 `Error: Validation Failed.` 的原因是文章对应的 URL 过长，因为 GitHub 规定不超过 50，至于为什么我也不知道。

怎么解决呢？先找到 comments.html 文件（GitHub Pages 和 Jekyll 搭建的博客系统）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190829113433116.png)

找到以下内容：

```js
        var gitalk = new Gitalk({
            id: decodeURI('{{ page.url }}'),
            clientID: '{{ site.gitalk.clientID }}',
            clientSecret: '{{ site.gitalk.clientSecret }}',
            repo: '{{ site.gitalk.repo }}',
            owner: '{{ site.gitalk.owner }}',
            admin: ['{{ site.gitalk.owner }}'],
            labels: ['gitment'],
            perPage: 50,
        })
```
注意在ID处加上 decodeURI 函数，可以把 page.url 为 `'/%E7%A8%8B%E5%BA%8F%E5%91%98/2019/08/27/java-url-urlconnection.html'` 转成 `"/程序员/2019/08/27/java-url-urlconnection.html"`，转了以后 id 的长度就变短了，然后问题就解决了。
