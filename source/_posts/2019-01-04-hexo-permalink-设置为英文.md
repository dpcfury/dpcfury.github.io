---
title: hexo permalink 设置为英文
date: 2019-01-04 21:04:07
categories:
- hexo
- permalink 优化技巧
tags: hexo 优化
---

>引言：最近分享给朋友本站一篇文章，发送链接的时候发现链接很长很难看：https://dpcfury.github.io/%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91/VPS%E4%BC%98%E5%8C%96/%E6%90%AC%E7%93%A6%E5%B7%A5openVZ%E8%BD%ACKVM%EF%BC%8Css%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8A%A0%E9%80%9F%E6%B5%8B%E8%AF%95.html/
>
>其主要原因是文章的title为中文，被转义之后成为了上面的编码。



于是查找到通过永久链接（[Permalink](https://hexo.io/zh-cn/docs/permalinks.html "Permalink")）的方式修改文章的url为英文，在hexo站点配置文件_config.yml中修改文章链接配置：

```
permalink: :year/:month/:day/:title/
permalink_defaults:
```

可见hexo默认在url中是以title结尾，而我们的文章title几乎都会包括中文，于是我们希望通过Front-matter中的变量替换title，通过这种方式实现自定义url，例如、

```
permalink: :category/:urlname.html/
```





