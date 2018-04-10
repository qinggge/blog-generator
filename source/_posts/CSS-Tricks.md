---
title: CSS-Tricks
date: 2018-04-08 22:53:47
tags: CSS 
---
# 前言
CSS是一个网页的表现，网页好不好看取决于CSS样式，而布局是CSS中比较重要的部分。本博文将总结个人在前端学习中碰到的几种布局及一些有趣的小技巧。  
# 左右布局  
## float  
浮动布局在当今页面中很常见，即是将需要布局的块级元素设置样式 `float: left;`，即可将元素向左浮动。  
<!-- more -->
e.g.  
元素设置：  
![fl_1](http://p69er22kd.bkt.clouddn.com/fl_1.png) ![fl_2](http://p69er22kd.bkt.clouddn.com/fl_2.png)  

初始元素及初始样式如上图，效果如下：  
![fl_3](http://p69er22kd.bkt.clouddn.com/fl_3.png)  

给 class `one` 及 `two` 加上样式 `float: left;` 后：  
![fl_4](http://p69er22kd.bkt.clouddn.com/fl_4.png)  

此时可见 div `wrapper` 的高度坍缩为0，这是因为子元素 `one` 及 `two` 设置浮动后，脱离了文档流，此时的最佳解决方法是，给父级元素 div `wrapper` 添加after伪类 `.clearfix` 即可：  
![fl_5](http://p69er22kd.bkt.clouddn.com/fl_5.png)  
![fl_6](http://p69er22kd.bkt.clouddn.com/fl_6.png)  

当然也可给 div `two` 设置样式 `float: right;`：  
![fl_7](http://p69er22kd.bkt.clouddn.com/fl_7.png)  

## flex布局  
一列定宽，一列自适应  
![flex_2](http://p69er22kd.bkt.clouddn.com/flex_2.png)  
![flex_1](http://p69er22kd.bkt.clouddn.com/flex_1.png)  

也可设置一列不定宽，一列自适应  
![flex_3](http://p69er22kd.bkt.clouddn.com/flex_3.png)  
![flex_4](http://p69er22kd.bkt.clouddn.com/flex_4.png)  

## inline-block  
将块级元素设置 `display: inline-block;` 也可作为页面布局。  
e.g.  
![ib_1](http://p69er22kd.bkt.clouddn.com/ib_1.png)  
![ib_2](http://p69er22kd.bkt.clouddn.com/ib_2.png)  

但是我们可以发现，两个元素之间出现了一个间距，这是因为元素间，换行显示或空格分隔的情况下会有间距，将元素间的空格或换行去除可以去掉这个间距：  
![ib_3](http://p69er22kd.bkt.clouddn.com/ib_3.png)  
![ib_4](http://p69er22kd.bkt.clouddn.com/ib_4.png)  

这个间距有时会对我们布局，或是兼容性处理产生影响。若需将此间距去除，可以参考博文[去除inline-block元素间间距的N种方法](http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/)里提到的数种方法，此处不细举例。

# 左中右布局  
## float
浮动布局也可以实现左中右三列布局，如下图：  
![fl_8](http://p69er22kd.bkt.clouddn.com/fl_8.png)  
![fl_9](http://p69er22kd.bkt.clouddn.com/fl_9.png)  

div `three` 设置向左或向右浮动都可以实现该布局。

## inline-block
同理，设置 `display: inline-block;`属性也可以实现左中右布局：  
![ib_5](http://p69er22kd.bkt.clouddn.com/ib_5.png)  
![ib_6](http://p69er22kd.bkt.clouddn.com/ib_6.png)  

## flex
使用flex布局可以十分简便地实现各种布局，可参照[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)及[Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)。  

# 元素的水平居中
## 块级元素
将块级元素的样式设置如下，即可设置块级元素水平居中（需设置宽度）：  
![hc_1](http://p69er22kd.bkt.clouddn.com/hc_1.png)  
![hc_2](http://p69er22kd.bkt.clouddn.com/hc_2.png)  

也可以把块级元素的的 `display` 属性设置为 `inline` ，即改为行内元素，这样通过设置父元素的 `text-align: center;`。来实现居中了，但这样做后块级元素就不能设置 `width` 属性了。
![hc_3](http://p69er22kd.bkt.clouddn.com/hc_3.png)  
![hc_4](http://p69er22kd.bkt.clouddn.com/hc_4.png)  

## 行内元素
将行内元素的父元素设置属性 `text-align: center;` 即可设置行内元素水平居中。  

# 元素的垂直居中
## 块级元素
可设置 `line-height`，设置块级元素为垂直居中：  
![vc_3](http://p69er22kd.bkt.clouddn.com/vc_3.png)  
![vc_4](http://p69er22kd.bkt.clouddn.com/vc_4.png)  

也可如下图设置：  
![vc_7](http://p69er22kd.bkt.clouddn.com/vc_7.png)  

## 行内元素
行内元素可设置 `line-height` 属性及 `height` 属性相等：  
![vc_9](http://p69er22kd.bkt.clouddn.com/vc_9.png)  
![vc_10](http://p69er22kd.bkt.clouddn.com/vc_10.png)  

# 一些不常见的CSS样式设置
1. 使用 `user-select: none;` 属性，可以禁止用户选中文本：<span class="select">我无法被选中</span>。  
2. 使用 `::selection` 选择器，可以使被选中的文本变化样式：<span class="eg">我被选中可以变色！！</span>（只能向 `::selection` 选择器应用少量 CSS 属性：`color`、`background`、`cursor` 以及 `outline`）。
3. 模糊文本设置：<span class="blur">我怎么这么模糊</span>，给元素添加上样式   
`color: transparent;`   
`text-shadow:  0 0 5px rgba(0,0,0,0.5);`  
即可。
<style>.eg::selection{color: red;}.select{user-select: none; font-weight: bold;}.blur{color: transparent; text-shadow: 0 0 5px rgba(0,0,0,0.5);}</style> 