---
title: 纯CSS制作小米logo图片
date: 2017-09-30 17:47:32
tags: CSS
comments: false
---

今天的主题是纯CSS制作小米的logo，尽量少用图片嘛，所以css技术和技巧要求很高哈

![](https://raw.githubusercontent.com/zuobaiquan/blogImg/master/CSS/小米logo6.png)

<!--more-->

```html
<div class="bg">
	<div class="M">
	</div>
</div>
```

先简单分析一波，这个图片可分为三个部分，M的外面+M里面的竖+一个大写的I
可以看到有一小段圆角，这个时候就要用到`border-radius` 
再回忆一下`border-radius`的用法及参数

>border-radius属性其实可以分为四个其他的属性：
>
>```
>border-radius-top-left /*左上角*/
>border-radius-top-right   /*右上角*/
>border-radius-bottom-right /*右下角*/
>border-radius-bottom-left   /*左下角*/
>```
>
>//提示：按顺时针方式

小米LOGO的圆角在右上角，那就设置`border-radius-top-right `为适当的值就OK，在那之前记得把设置`border-bottom：none`,不能设置透明色 
如果设置透明色就会出现这样的效果 

![](https://raw.githubusercontent.com/zuobaiquan/blogImg/master/CSS/%E5%B0%8F%E7%B1%B3logo2.png)

显然我们想要的不是这样的，为什么会出现这样的效果呢？
我们来看一下边框到底是怎样的

```css
border-right: 30px solid red;
border-left: 30px solid green; 
border-top: 30px solid blue; 
border-bottom: 30px solid teal;
```

它的效果图是这样的（反相了） 

![](https://raw.githubusercontent.com/zuobaiquan/blogImg/master/CSS/%E5%B0%8F%E7%B1%B3logo3.png)

虽然配色有点辣眼睛，但是可以看出边框的交界是一个斜角，所以把底边设置透明就会出现两个小角角，
设置为`none`就会变成一个直的脚了，但是设置为`none`对浏览器IE6，IE7不兼容，所以最好设置为`border-bottom：0` 
代码如下：

```css
.M {
    width: 80px;  
    height: 80px;  
    border-radius: 0 45px 0 0;  
    border-top: 30px solid white;  
    border-right: 30px solid white;  
    border-left: 30px solid white;
    border-bottom-width: 0;
}

```

此时的效果是这样的

![](https://raw.githubusercontent.com/zuobaiquan/blogImg/master/CSS/小米logo4.png)

很好，完成了一大半，剩下的就是两条竖线了，这时有两种方法：

1. 再创建两个div或者其他的标签来设置样式
2. 利用CSS3中的伪元素来制作 
   由于创建另外的标签来制作基本上人人都会，就在此跳过，我们用伪元素来做，那么，什么是伪元素呢？

你一定知道伪类样式，比如hover，在做效果的时候经常用到，而hover是不会改变元素本身的样式的，鼠标移开后元素又恢复成原来的样子，这就给人一个假象，元素样式改变了，其实并没有，那伪元素也是一样，我们可以添加额外元素而不扰乱文档本身，这就是“伪元素”。伪元素:`before`和:`after`二者的作用为在元素之前或之后插入某些内容。（伪元素和伪类样式不一样，要注意用两个冒号连接） 
此时，在M的外框里加入伪元素，代码如下：

```css
.M::before {
	content: "";
	display: inline-block;
	position: relative;
	left: 25px;
	top: 25px;
	height: 55px;
	width: 30px;
	background-color: white;
}     
```

此时的效果图如下：

![](https://raw.githubusercontent.com/zuobaiquan/blogImg/master/CSS/小米logo5.png)

此时已经完成一大半，还差个I，话不多说，搞起！ 
创建一个`after`伪元素并且调整样式：

```css
.M::after{
  content: " ";
  display: inline-block;
  position: relative;
  left: 110px;
  top: -30px;
  height: 110px;
  width: 30px;
  background-color: white;
}
```

设置了这个I的伪元素后发现前面的`before`伪元素的位置也发生了变化，
调整后代码如下：

```css
.M::before {
	content: "";
	display: inline-block;
	position: relative;
	left: 25px;
	top: -30px;
	height: 55px;
	width: 30px;
	background-color: white;
}  
```

此时，小米的logo就完成了，效果图

![](https://raw.githubusercontent.com/zuobaiquan/blogImg/master/CSS/小米logo6.png)