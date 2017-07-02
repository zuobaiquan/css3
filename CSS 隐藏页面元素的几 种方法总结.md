---
title: CSS 隐藏页面元素的 几 种方法总结
date: 2016-01-11 12:07:49
tags: CSS
comments: false
---

用 CSS 隐藏页面元素有许多种方法。你可以将 `opacity` 设为 `0`、将 `visibility` 设为 `hidden`、将 `display` 设为 `none` 或者将 `position` 设为 `absolute` 然后将位置设到不可见区域。<!-- more -->

你有没有想过，为什么我们要有这么多技术来隐藏元素，而它们看起来都实现的是同样的效果？**每一种方法实际上与其他方法之间都有一些细微的不同**，这些不同决定了在一个特定的场合下使用哪一个方法。我们只需要记住的细小不同点，根据不同情况选择上面这些方法中适合的方法来隐藏元素。

## display

`display` 属性依照词义真正隐藏元素。将 `display` 属性设为 `none` 确保元素不可见并且连盒模型也不生成。使用这个属性，**被隐藏的元素不占据任何空间**。不仅如此，一旦`display` 设为 `none` 任何对该元素直接打用户交互操作都不可能生效。此外，读屏软件也不会读到元素的内容。这种方式产生的效果就像元素完全不存在。

任何这个元素的子孙元素也会被同时隐藏。为这个属性添加过渡动画是无效的，它的任何不同状态值之间的切换总是会立即生效。

不过请注意，通过 DOM 依然可以访问到这个元素。因此你可以通过 DOM 来操作它，就像操作其他的元素。

```
.hide {
   display: none;
}
```

看下面的例子：

[@SitePoint](http://codepen.io/SitePoint) 提供的例子[“用 display 隐藏元素”](http://codepen.io/SitePoint/pen/zBGbjb/)

你将看到第二个块元素内有一个 `<p>` 元素，它自己的 `display` 属性被设置成`block`，但是它依然不可见。这是 `visibility:hidden` 和 `display:none` 的另一个不同之处。在前一个例子里，将任何子孙元素 `visibility` 显式设置成 `visible` 可以让它变得可见，但是 `display` 不吃这一套，不管自身的 `display` 值是什么，只要祖先元素的 `display` 是 `none`，它们就都不可见。

现在，将鼠标移到第一个块元素上面几次，然后点击它。这个操作将让第二个块元素显现出来，它其中的数字将是一个大于 0 的数。这是因为，元素即使被这样设置成对用户隐藏，还是可以通过 JavaScript 来进行操作。

 

## Visibility

第二个要说的属性是 `visibility`。将它的值设为 `hidden` 将隐藏我们的元素。如同`opacity` 属性，被隐藏的元素依然会对我们的网页布局起作用。与 `opacity` 唯一不同的是它不会响应任何用户交互。此外，元素在读屏软件中也会被隐藏。

这个属性也能够实现动画效果，只要它的初始和结束状态不一样。这确保了 visibility 状态切换之间的过渡动画可以是时间平滑的（事实上可以用这一点来用 hidden 实现元素的[延迟显示和隐藏](http://www.zhangxinxu.com/wordpress/2013/05/transition-visibility-show-hide/)——译者注）。

```
.hide {
   visibility: hidden;
}
```

下面的例子演示了 `visibility` 与 `opacity` 有怎样的不同：

看 [@SitePoint](http://codepen.io/SitePoint) 提供的例子[“用 visibility 隐藏元素”](http://codepen.io/SitePoint/pen/pbJYpV/)

注意，如果一个元素的 `visibility` 被设置为 `hidden`，同时想要显示它的某个子孙元素，只要将那个元素的 `visibility` 显式设置为 `visible` 即可（就如例子里面的 .o-hide p——译者注）。尝试只 hover 在隐藏元素上，不要 hover 在 p 标签里的数字上，你会发现你的鼠标光标没有变成手指头的样子。此时，你点击鼠标，你的 click 事件也不会被触发。

而在 `<div>` 标签里面的 `<p>` 标签则依然可以捕获所有的鼠标事件。一旦你的鼠标移动到文字上，`<div>` 本身变得可见并且事件注册也随之生效。

## Opacity

`opacity` 属性的意思是设置一个元素的透明度。它不是为改变元素的边界框（bounding box）而设计的。**这意味着将 opacity 设为 0 只能从视觉上隐藏元素**。**而元素本身依然占据它自己的位置并对网页的布局起作用。它也将响应用户交互。**

```
.hide {
  opacity: 0;
}
```

 

如果你打算使用 `opacity` 属性在读屏软件中隐藏元素，很不幸，你并不能如愿。元素和它所有的内容会被读屏软件阅读，就像网页上的其他元素那样。换句话说，元素的行为就和它们不透明时一致。

我还要提醒一句，`opacity` 属性可以用来实现一些效果很棒的动画。任何 `opacity` 属性值小于 `1` 的元素也会创建一个新的堆叠上下文（[stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)）。

看下面的例子：

看 [@SitePoint](http://codepen.io/SitePoint) 提供的例子[“用 opacity 隐藏元素”](http://codepen.io/SitePoint/pen/bedZrR/)

当你的鼠标移到被隐藏的第 2 个的区块上，元素状态平滑地从完全透明过渡到完全不透明。区块也将 `cursor` 属性设置为了 `pointer`，这说明了用户可以与它交互。

 

## Position

假设有一个元素你想要与它交互，但是你又不想让它影响你的网页布局，没有合适的属性可以处理这种情况（opacity 和 visibility 影响布局， display 不影响布局但又无法直接交互——译者注）。在这种情况下，你只能考虑将元素移出可视区域。这个办法既不会影响布局，有能让元素保持可以操作。下面是采用这种办法的 CSS：

```
.hide {
   position: absolute;
   top: -9999px;
   left: -9999px;
}
```

 

下面的例子阐明了怎样通过绝对定位的方式隐藏元素，并让它和前面的那个例子效果一样：

看 [@SitePoint](http://codepen.io/SitePoint) 提供的例子[“用 position 属性隐藏元素”](http://codepen.io/SitePoint/pen/QEboZm/)

这种方法的主要原理是通过将元素的 top 和 left 设置成足够大的负数，使它在屏幕上不可见。采用这个技术的一个好处（或者潜在的缺点）是用它隐藏的元素的内容可以被读屏软件读取。这完全可以理解，是因为你只是将元素移到可视区域外面让用户无法看到它。

你得避免使用这个方法去隐藏任何可以获得焦点的元素，因为如果那么做，当用户让那个元素获得焦点时，会导致一个不可预料的焦点切换。这个方法在创建自定义复选框和单选按钮时经常被使用。（用 DOM 模拟复选框和单选按钮，但用这个方法隐藏真正的 checkbox 和 radio 元素来“接收”焦点切换——译者注）

## Clip-path

隐藏元素的另一种方法是通过剪裁它们来实现。在以前，这可以通过 `clip` 属性来实现，但是这个属性被废弃了，换成一个更好的属性叫做 `clip-path`。Nitish Kumar 最近在 SitePoint 发表了[“介绍 `clicp-path` 属性”](https://www.sitepoint.com/introducing-css-clip-path-property/)这篇文章，通过阅读它可以了解这个属性的更多高级用法。

记住，`clip-path` 属性还[没有在 IE 或者 Edge 下被完全支持](http://caniuse.com/#feat=css-clip-path)。如果要在你的 `clip-path` 中使用外部的 SVG 文件，浏览器支持度还要更低。使用 `clip-path` 属性来隐藏元素的代码看起来如下：

```
.hide {
  clip-path: polygon(0px 0px,0px 0px,0px 0px,0px 0px);
}
```

 

下面是一个实际使用它的例子：

看 [@SitePoint](http://codepen.io/SitePoint) 提供的例子[“用 clip-path 属性隐藏元素”](http://codepen.io/SitePoint/pen/YWXgdW/)

如果你把鼠标悬停在第一个元素上，它依然可以影响第二个元素，尽管第二个元素已经通过`clip-path` 隐藏了。如果你点击它，它会移除用来隐藏的 class，让我们的元素从那个位置显现出来。被隐藏元素中的文字仍然能够通过读屏软件读取，许多 WordPress 站点使用`clip-path` 或者之前的 `clip` 来实现专门为读屏软件提供的文字。

虽然我们的元素自身不再显示，它也依然占据本该占据的矩形大小，它周围的元素的行为就如同它可见时一样。记住用户交互例如鼠标悬停或者点击在剪裁区域之外也不可能生效。在我们的例子里，剪裁区大小为零，这意味着用户将不能与隐藏的元素直接交互。此外，这个属性能够使用各种过渡动画来实现不同的效果。

## 结论

在这篇教程里，我们看了 5 种不同的通过 CSS 隐藏元素的方法。每一种方法都与其他几种有一点区别。知道你想要实现什么有助于你决定采用哪一个属性，随着时间推移，你就能根据实际需求本能地选择最佳方式了。

转自：http://www.zcfy.cc/article/457