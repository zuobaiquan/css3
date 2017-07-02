---
title: CSS实现背景透明，文字不透明(兼容所有浏览器)
date: 2016-08-11 12:07:49
tags: CSS
comments: false
---

我们平时所说的调整透明度，其实在样式中是调整不透明度，如下图所示例：
<!-- more -->
![此处输入图片的描述][1]
打开ps，在图层面板上，可以看到设置图层整理不透明度的菜单，从 0% （完全透明）到 100%（完全不透明）。
![此处输入图片的描述][2]
实现透明的css方法通常有以下3种方式，以下是不透明度都为80%的写法

css3的opacity:x，x 的取值从 0 到 1，如opacity: 0.8
css3的rgba(red, green, blue, alpha)，alpha的取值从 0 到 1，如rgba(255,255,255,0.8)
IE专属滤镜 filter:Alpha(opacity=x)，x 的取值从 0 到 100，如filter:Alpha(opacity=80)
css3的opacity

兼容性：IE6、7、8不支持，IE9及以上版本和标准浏览器都支持
![此处输入图片的描述][3]
 使用说明：设置opacity元素的所有后代元素会随着一起具有透明性，一般用于调整图片或者模块的整体不透明度
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>opacity</title>
<style>
*{
    padding: 0;
    margin: 0;
}
body{
    padding: 50px;
    background: url(img/bg.png) 0 0 repeat;
}
.demo{
  padding: 25px;
  background-color:#000000;
  opacity: 0.2;
}
.demo p{
    color: #FFFFFF;
}
</style>
</head>
<body>    
<div class="demo">
    <p>背景透明，文字也透明</p>
</div>
</html>
```
使用opacity后整个模块都透明了，展现如下：
![此处输入图片的描述][4]
那么使用opacity实现《背景透明，文字不透明》是不可取的。
css3的rgba
兼容性：IE6、7、8不支持，IE9及以上版本和标准浏览器都支持
![此处输入图片的描述][5]
使用说明：设置颜色的不透明度，一般用于调整background-color、color、box-shadow等的不透明度。
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>css3的rgba</title>
<style>
*{
    padding: 0;
    margin: 0;
}
body{
    padding: 50px;
    background: url(img/bg.png) 0 0 repeat;
}
.demo{
  padding: 25px;
  background-color:#000000;/* IE6和部分IE7内核的浏览器(如QQ浏览器)下颜色被覆盖 */
  background-color:rgba(0,0,0,0.2); /* IE6和部分IE7内核的浏览器(如QQ浏览器)会读懂，但解析为透明 */
}
.demo p{
    color: #FFFFFF;
}
</style>
</head>
<body>    

<div class="demo">
    <p>背景透明，文字也透明</p>
</div>
</html>
```
在background-color中使用rgba，标准浏览器中，背景透明，文字不透明，展现如下：
![此处输入图片的描述][6]
很奇葩的是，IE6和部分IE7内核的浏览器(如QQ浏览器)会读懂rgba，解析后颜色为透明，其实应该是null
![此处输入图片的描述][7]
那么使用opacity实现背景透明，文字不透明是可取的。

IE专属滤镜 filter:Alpha(opacity=x)

使用说明：IE浏览器专属，问题多多，本文以设置背景透明为例子，如下：

1、仅支持IE6、7、8、9，在IE10版本被废除
2、在IE6、7中，需要激活IE的haslayout属性(如：*zoom:1或者*overflow:hidden)，让它读懂filter:Alpha
3、在IE6、7、8中，设置了filter:Alpha的元素，父元素设置position:static(默认属性)，其子元素为相对定位，可让子元素不透明
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>opacity</title>
<style>
*{
    padding: 0;
    margin: 0;
}
body{
    padding: 50px;
    background: url(img/bg.png) 0 0 repeat;
}
.demo{
  padding: 25px;
  background: #000000;
  filter:Alpha(opacity=50);/* 只支持IE6、7、8、9 */
  position:static; /* IE6、7、8只能设置position:static(默认属性) ，否则会导致子元素继承Alpha值 */
  *zoom:1; /* 激活IE6、7的haslayout属性，让它读懂Alpha */
}
.demo p{
    color: #FFFFFF;
    position: relative;/* 设置子元素为相对定位，可让子元素不继承Alpha值，保证字体颜色不透明 */
}      
</style>
</head>
<body>    
<div class="demo">
    <p>背景透明，文字不透明</p>
</div>
```
全兼容的方案

上以上3点分析可知，设置透明背景文字不透明，可采用的属性有rgba和IE的专属滤镜filter:Alpha，其兼容性如下图所示：
![此处输入图片的描述][8]
针对IE6、7、8浏览器，我们可以采用fiter:Alpha，针对标准浏览器我们采用rgba，那么问题来了，IE9浏览器2个属性都支持，一起使用会重复降低不透明度...

那么，如何只对IE6、7、8使用fiter:Alpha如何实现呢？
```css
/* 只支持IE6、7、8 */

@media \0screen\,screen\9 {...}
```
ok，所有问题都解决了，全部代码如下：
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>背景透明，文字不透明</title>
<style>
*{
    padding: 0;
    margin: 0;
}

body{
  padding: 50px;
  background: url(img/bg.png) 0 0 repeat;
}

.demo{
  padding: 25px;
  background-color: rgba(0,0,0,0.5);/* IE9、标准浏览器、IE6和部分IE7内核的浏览器(如QQ浏览器)会读懂 */
}
.demo p{
  color: #FFFFFF;
}
@media \0screen\,screen\9 {/* 只支持IE6、7、8 */
  .demo{
    background-color:#000000;
    filter:Alpha(opacity=50);
    position:static; /* IE6、7、8只能设置position:static(默认属性) ，否则会导致子元素继承Alpha值 */
    *zoom:1; /* 激活IE6、7的haslayout属性，让它读懂Alpha */
  }
  .demo p{
    position: relative;/* 设置子元素为相对定位，可让子元素不继承Alpha值 */
  }  
}

</style>
</head>
<body>    
<div class="demo">
    <p>背景透明，文字不透明</p>
</div>
</html>
```
 我们看到这个可能会觉得很复杂，为什么不直接用两个DIV放在同一个位置就行了，一个不透明DIV，一个文字DIV，很简单的解决问题，这也是好个方法，但是需要写绝对定位或负margin，并出现空内容的DIV，而且这是这种方法在有些场景下也会显得复杂，如下示例，鼠标经过商标后展现展现透明的提示文案，就需要控制2个DIV啦~
 ![此处输入图片的描述][9]


  [1]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714144736561-863618505.png
  [2]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714144830639-716499190.jpg
  [3]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714145037639-681202916.png
  [4]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714145214889-941141268.png
  [5]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714145308061-1256426158.png
  [6]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714145427311-422392724.png
  [7]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714145634561-581154038.png
  [8]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714145832186-131440309.png
  [9]: http://images2015.cnblogs.com/blog/986385/201607/986385-20160714150136357-432087096.gif