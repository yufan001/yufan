---
title: 'input 属性为 number，maxlength不起作用如何解决？ '
layout: post
tags: 笔记,前端,心得
---


`<input type="text"  maxlength="5" /> `
效果ok，当` <input type="number"  maxlength="5" />`时maxlength失效，长度可以无限输入。
解放方案：
```
<input type="number" oninput="if(value.length>5)value=value.slice(0,5)" />
```