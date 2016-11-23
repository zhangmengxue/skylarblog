title: 跨终端开发必备概念汇总
date: 2015-01-20 23:03:17
tags:
- 技术文
---

移动大潮已然到来，跟上节奏是必须。在进行更进一步深入学习和研究各种相关问题之前，有些概念是必须研究透彻的，这将会是以后开发和适配的基石。

## 文章导读
	一.单位英寸像素数（Pixel Per Inch，PPI）
	二.设备像素比率（Device Pixel Ratio，DPR）
	三.设备像素/css像素
	四.各路单位扫盲＋分析
	五.viewport相关
	六.width和device－width

<!-- more --> 
## 一.单位英寸像素数（Pixel Per Inch，PPI）

PPI是一个度量单位，是像素密度单位，它表示了现实世界一英寸内的像素数，因此它决定了屏幕的显示质量。

像我们常说的1024*768其实是指的屏幕分辨率，对应在现实中的分辨率就用PPI作为度量单位，可以举个例子来说明：

　　｜－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－｜

　　｜如果显示器的屏幕大小是14英寸（对角线的长度）纵横比是3:4－－>

　　｜那么水平方向上的长度就为：14*（4/5）＝11.2 英寸  （1英寸＝0.762寸）

　　｜从而我们常说的1024对应现实世界的分辨率就为：1024/11.2 ＝ 91.4PPI

　　｜－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－｜

所以通常要分辨清楚，像我们说的1024*768和iPhone5s的640*1136他们不是对应PPI这个单位的。

正是因为不相同，存在这样的对应关系，所以在终端上必要的时候就会做PPI的适配，以Retina屏为例子：

－－>我们不想文字在Retina屏上变小（因为在相同尺寸的屏幕下，Retina屏拥有比非Retina屏更多的物理像素，就是我们现实世界中的像素数，所以相同大小的字在Retina下会变小），所以为了做适配，我们可以采用类似媒体查询的办法来改变字体的大小。

　　｜－PS－｜：Retina显示屏

　　　　定义：就是具备足够高像素密度而使到人体肉眼无法分辨其中单独像素点的液晶屏，就像最早的iPhone4的屏幕。它的屏幕分辨率为960×640（每英寸像素数326ppi）。

　　　　我觉得最大的区别就是：因为我们看到的图像都是通过物理像素呈现的，所以像素越密集图像就越清晰，而所谓的Retina显示屏就是在相同尺寸的屏幕下比非Retina的屏幕具备更多的物理像素。

##二.设备像素比率（Device Pixel Ratio，DPR）

定义：

　　　　devicePixelRatio指的是window.devicePixelRatio, 被所有WebKit浏览器以及Opera所支持，window.devicePixelRatio是设备上物理像素和设备独立像素(device-independent pixels (dips))的比例，即：devicePixelRatio=设备物理像素/设备独立像素。

　　　　它表示的就是物理像素与逻辑像素（px）的对应关系。

举个例子：　　　　

　　　　以iPhone为例，retina下devicePixelRatio=1，非retina下devicePixelRatio=2，这是因为无论是retina还是非retina屏，竖屏下，设备独立像素都是320，而它们的设备物理像素分别为640、320。

　　这边还有更加详细的相关阅读：[设备像素比devicePixelRatio的简单介绍](http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/)  [devicePixelRatio＝1.5引发的问题](https://www.imququ.com/post/devicepixelratio-and-border-width.html)

　　于是也就有了DPR适配的问题，因为存在设备像素比率这个东西，所以当我们遇到那个经典的1px问题（无非就是设计师希望在任何的屏幕上都有一条1物理像素的边），要做的就多了。

　　[手机淘宝的flexible设计与实现](http://www.html-js.com/article/2402)

## 三.设备像素/css像素

要先说下分辨率（Resolution），它就是指屏幕区域的宽高所占的像素数。分辨率说全了就是设备的分辨率，这两个是一回事。这个分辨率是指每英寸的面积上可产生的像素点，分辨率越高代表可以将画面显示的更精细（有时候获取的显示器分辨率，其实是指桌面设定的分辨率，而不是显示器的物理分辨率。大多数情况下，我们设置的分辨率与物理分辨率一致，显示效果才最佳）。

其实这里说的分辨率（Resolution）在绝大多数的情况下跟我们题目上的设备像素是一个东西，名字的不同而已。

　　好，下面就专心说设备像素/css像素。

　　1.我们姑且认定设备的pixels为正确（标准）的pixels宽度。这些pixels决定了你工作所用的那些设备上正式的分辨率。在大多数情况下，能够从screen.width/height上取出具体值；

　　2.现代浏览器上的缩放，是基于“伸展”pixels。结果是，html元素上的宽度并没有因为缩放200%而由128pix变成256px，而是真实的pixels的被计算成了双倍。html元素在形式上依然是128CSS的pixels，即便它占用了256设备的pixels；

　　3.图例：

　　下列图片将清楚的解释这个概念。如图,有4个1像素，缩放为100%的html元素，CSS的pixels完整的和设备的pixels重叠

![图1](/img/1.png)

　　当我们缩小浏览器时，CSS的pixels开始收缩，导致1单位的设备的pixels上重叠了多个CSS的pixels，如下图:

![图2](/img/2.png)

　　同理，放大浏览器时，相反的事情发生了，CSS的pixels开始扩大，导致1单位的CSS的pixels上重叠了多个设备的pixels，如下图：　　　　　

![图3](/img/3.png)

　　4.总体而言，你只需要关注CSS的pixels，这些pixels指定你的样式被如何渲染，设备的pixels几乎对你毫无用处。但对用户而言却不是这样。用户会缩放页面，直到他能舒服的阅读内容。但是你不需关心这些缩放级别。浏览器会自动的保证你的CSS的pixels会被伸展还是收缩；

　　5.在缩放级别为100%时，1单位的CSS的pixel是严格相等于1单位的设备pixel。

　　6.上面有说道，页面的清晰和模糊其实可以通过设备像素和css像素的关系来解释。那么这里也要注意下Retina屏，不然稍不注意，图可能会糊掉。

　　　　非retina屏下，一个css像素“覆盖”在一个设备物理像素上；
　　　　retina屏下，一个css像素“覆盖”在四个设备物理像素上，实质是一个css像素被瓜分成了四份，效果就变糊了。

　　　　原理就像下面这样：

![图4](/img/4.png)

## 四.各路单位扫盲＋分析

　1.em

　　　　em是相对单位，参考物是父元素的font-size，同百分比方式的单位相同，具有继承的特点，如果字体大小是16px（浏览器的默认值），那么 1em = 16px。 如果父元素设置了font-size:62.5%，那么相应的1em=10px了。

　　　　这里有篇文章研究的比较仔细：[关于webapp中的文字单位的一些倒腾](http://www.html-js.com/article/2400)

　　2.rem

　　　　rem也是相对单位，不过总是相对于根元素的字体大小，也就是<html>元素。

　　　　现在越来越多的人采用rem在不同的设备间进行布局了，可以通过js动态的改变<html>元素的font-size，或是通过媒体查询来动态改变<HTML>元素的font-size的大小，来适配不同设备的不同屏幕大小，实现跨终端的目的，而且支持度也越来越好了。

　　3.vw和vh

　　　   vw Viewport宽度， ** 1vw ** 等于viewport宽度的1%；

　　　　vh Viewport高度， ** 1vh ** 等于viewport高的的1%；

　　4.vmin和vmax

　　　　vw和vh对应于viewport的width和height，而vmin和vmax分别对应于width、height中的最小值和最大值，例如如果浏览器的宽/高被设置为1000px/600px，那么

　　　　1vmin = 600 * 1/100;

　　　　1vmax = 1000 * 1/100



[MSDN上有更为详尽的解释](http://msdn.microsoft.com/en-us/magazine/jj687762.aspx)

## 五.viewport相关

｜－视口viewport（这里且先说pc相关的viewport）－｜

　　viewport的定义是严格的等于浏览器的窗口，它不是一个HTML的概念，所以我们不能通过css去修改它。

　　实际上，viewport的功能在于控制网站的最高block容器，那就是<html>元素。也就是说<html>元素为viewport宽度的100%。虽然我们知道document.documentElement实际上就是<html>元素，但是document.documentElement. clientWidth/Height给出的实际是viewport的尺寸，而不是<html>元素，不管<html>元素的尺寸如何改变，都只会得到viewport的尺寸。那我们该如何获取<html>元素的尺寸呢，可以使用document.documentElement.offsetWidth/Height，但还是有bug，IE用这个值标示viewport的尺寸而非<html> 。

｜－viewport（然后放开了的说下我学习和理解到的跨终端相关的viewport）－｜

1.两种viewport：

　　　　虚拟的viewport＃visualviewport －－visualviewport是当前显示在屏幕上的部分页面。用户会滚动页面来改变可见部分，或者缩放浏览器来改变visualviewport的尺寸。window.innerWidth/Height返回visualviewport的尺寸。

![图5](/img/5.png)

　　　　布局的viewport＃layoutviewport－－css布局通常都是按照layoutviewport来定义，而且比visualviewport宽很多。同时<html>元素的宽度也是继承于layoutviewport。那么layoutviewport有多宽呢？每个浏览器都不同。iPhone上的Safari使用980px、Opera 850px，安卓的Webkit核心800px，IE974px。浏览器已经选择好他们的layoutviewport的尺寸，它完整的覆盖了最小缩放模式下的移动浏览器的屏幕。document.documentElement.clientWidth/Height传递layoutviewport的尺寸。


![图6](/img/6.png)
　　

2.和pc浏览器一样，screen.width/height标示了设备屏幕的尺寸，以设备的pixels度量。如果需要知道当前visualviewport相对于layoutviewport的距离。这就是滚动位移，如同在桌面浏览器一样，使用window.pageX/YOffset存储。和在桌面系统一样,document.documentElement.offsetWidth/Height给出了<html>元素以CSS的pixels度量的尺寸。

　　上面内容只是基本，并不完整，不同浏览器的兼容问题还需要进一步研究，详细阅读请看：[A Table of two viewports－part one](http://www.quirksmode.org/mobile/viewports.html)       当然第二部分也非常值得一看： [A Table of two viewports－part two](http://www.quirksmode.org/mobile/viewports2.html)　

　　quirksmode上的一个非常全的viewport相关的兼容性表，可用于适配时查看：[http://www.quirksmode.org/mobile/tableViewport.html](http://www.quirksmode.org/mobile/tableViewport.html)

　　其中涉及到的页面元素大小偏移相关我之前也有整理,没那么深入，倒也通俗易懂： 页面元素坐标和偏移相关整理

## 六.width和device－width

我了解到的定义是：

　　　　+－－width/height使用以CSS的pixels度量的layoutviewport，通俗的说就是页面内容的宽度；

　　　　+－－device-width/height使用以设备的pixels度量的设备屏幕（device screen）；

　　　　#－－width/height 反映document. documentElement. clientWidth/Height的值；

　　　　#－－device-width/height 反映screen.width/height；

　　　　但上面只是普遍情况，如果稍在ios设备下尝试下，就能发现事实跟上面说的不一样。

　　　　在某些设备下：screen.width == 设备的物理分辨率 / window.devicePixelRatio。具体情况还需要详细的测试。



上面也还只是列出了相关问题的基本了解与学习，在这个过程中更加发现，需要深入研究与学习的知识点还是很多的。

比如说<META>标签相关，还有1px的问题，都是值得深入学习的。

后续继续持续学习，逐步完善。
　
