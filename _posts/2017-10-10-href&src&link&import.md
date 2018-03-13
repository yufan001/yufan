---
title: 'href和src的一点区别'
layout: post
tags:
    - href
    - src
    - url
---



总结资料以及理解的一些href和src的区别以及link和@import的区别

<!--more-->

#####  href是Hypertext Reference的缩写，表示超文本引用。用来建立当前元素和文档之间的链接。常用的有：link、a.

```html
<link href="reset.css" rel="stylesheet"/>  
```

	>  浏览器会识别该文档为css文档，并下载该文档，但是不会停止对当前文档的处理。（这也是一般使用link而不是@import加载css的原因）




##### src是source的缩写，src的内容是页面必不可少的一部分，是引入。

src指向的内容会嵌入到文档中当前标签所在的位置。常用的有：img、script、iframe等。

``` html
<script src="script.js"></script>
```

	>  当浏览器解析到该元素时，会暂停浏览器的渲染，知道该资源加载完毕。(这也是将js脚本放在底部而不是头部得原因。)



### 补充：link和@import的区别

两者都是外部引用CSS的方式，但是存在一定的区别：

* link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。
* link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
* link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
* ink支持使用Javascript控制DOM去改变样式；而@import不支持。













