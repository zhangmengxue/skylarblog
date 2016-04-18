title: 相识HTML5 canvas
date: 2015-02-25 22:58:44
tags:
- 博客
---

Canvas是HTML的API，我们可以用它在网页中实时的来生成图像。

文章导读

　　1.必备技能

　　2.用于画图的函数

　　　　例子：-会话气泡- -心形- -钟表- -星球里的星星-  -调色板- -鼠标绘图-  -旋转的小方块-

　　3.图像的处理

　　　　例子：

　　　　　　-图像的灰度和翻转效果- -拾色器-  -放大镜- -图像的高斯模糊-　

### 一、必备技能

这是一个canvas标签：

	<canvas id="c1" width="400" height="400">
		<span>注意：不支持canvas的浏览器就会显示这个span标签中的内容,并且在css中定义canvas的宽高是等比例缩放的，canvas中行内的宽高样式才是画布的大小</span>
	</canvas>

控制它的宽高是要写在行内样式中的，像上面那样。这样我们就有了一个canvas元素，然后我们就可以去操作它了：

	var canvas = document.getElementById('Canvas');

	if (canvas.getContext) {
	    var content = canvas.getContext('2d');
	}

获取元素的同时，还要获取canvas的2D绘图环境。要是用于3D绘图的话，就要用WebGL了。

### 二、用于画图的函数

然后，我们要开始在画布上绘图了。它的画布是这样的网格：

![图1](/img/canvas.png) 

**2.1矩形**

canvas中只支持这一种形状的函数，别的形状就都要自己组合来实现了。

	fillRect(x, y, width, height) //画一个填充的矩形
	strokeRect(x, y, width, height) //画一个只有边框的矩形
	clearRect(x, y, width, height) //清除指定的矩形区域

其中的（x,y）是指矩形左上角的坐标。

[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E7%9F%A9%E5%BD%A2.html)
[运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E7%9F%A9%E5%BD%A2.html)

**2.2路径**

	1.首先，创建一条路径－－beginPath() 
	2.然后，通过一些绘图的方法做一些绘图操作
	3.然后关闭路径－－closePath()
	4.我们已经把路径创建好了，然后就是填充或者绘制路径到我们的画布上－－stroke()/fill()

比如说画一个三角形：[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E4%B8%89%E8%A7%92%E5%BD%A2.html)    [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E4%B8%89%E8%A7%92%E5%BD%A2.html)

需要注意的是其中两个三角形，颜色不同，一不小心可能会出问题，所以我们有时可能需要save()和restore()函数的帮助来达到局部作用的效果。
canvas还提供了两个移动画笔的函数：

	moveTo(x, y) //将画笔移动到指定的(x,y)的位置
	lineTo(x, y) //在当前画笔的位置到指定的(x,y)位置画一条线

尝试一下moveTo()方法我们可以画个笑脸：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E4%B8%AA%E7%AC%91%E8%84%B8.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E4%B8%AA%E7%AC%91%E8%84%B8.html)

结合使用moveTo和lineTo方法，我们可以实现鼠标像画笔一样在画板上绘图的效果：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E9%BC%A0%E6%A0%87%E7%BB%98%E5%9B%BE.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E9%BC%A0%E6%A0%87%E7%BB%98%E5%9B%BE.html)

然后我们严格的按照上面说的规范步骤，1，2，3，4并结合画圆的函数，可以自己绘制一个钟表，稍加改进就是个嘀嗒嘀嗒走的时钟了：
[查看代码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E9%92%9F%E8%A1%A8.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E9%92%9F%E8%A1%A8.html)

 **2.3文字**

 	fillText('要显示的文本'，x坐标，y坐标)此方法不支持文本断行，要显示多行文本只能多次调用

通过fillText()方法我们可以在canvas画布上写文字：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E6%96%87%E6%9C%AC.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E6%96%87%E6%9C%AC.html)

**2.4圆形和扇形**

	ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);

	(x,y)－－圆心坐标
	radius－－半径
	startAngle－－扇形的起始角度（弧度）
	endAngle－－扇形的终止角度（弧度）
	anticlockwise－－做图时是顺时针画（true）还是逆时针（false）画

[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E5%9C%86%E5%BD%A2%E5%92%8C%E6%89%87%E5%BD%A2.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E5%9C%86%E5%BD%A2%E5%92%8C%E6%89%87%E5%BD%A2.html)

结合对rgb颜色的相关操作，我们可以做出类似调色板的效果：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E8%B0%83%E8%89%B2%E6%9D%BF.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E8%B0%83%E8%89%B2%E6%9D%BF.html)

**2.5不规则图形**

我们可以通过画曲线来得到不规则的图形，canvas为我们提供了两个高大上的函数：

	－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
	#quadraticCurveTo(cp1x, cp1y, x, y)
	(cp1x,cp1y)－－控制点
	此函数表示从当前的画笔位置到（x，y）画一条二次曲线
	－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
	#bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
	(cp1x,cp1y)－－控制点1  (cp2x,cp2y)－－控制点2
	此函数表示从当前的画笔位置到（x，y）画一条bezier曲线
	－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

这两个曲线具体含义我也不太清楚，但大概是这个样子：

![图2](/img/canvas2.png)

画个会话气泡试一下quadraticCurveTo函数：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E4%B8%AA%E4%BC%9A%E8%AF%9D%E6%B0%94%E6%B3%A1.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E4%B8%AA%E4%BC%9A%E8%AF%9D%E6%B0%94%E6%B3%A1.html)

画个心形试一下bezierCurveTo函数：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E7%94%BB%E5%BF%83%E5%BD%A2.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E7%94%BB%E5%BF%83%E5%BD%A2.html)

**2.6渐变和阴影**

	＃渐变：
	var myGradient = ctx.createLinearGradient(x1, y1, x2, y2); 
	(x1,y1)－－起点坐标  (x2,y2)－－终点坐标
	myGradient.addColorStop(0, "#BABABA"); 
	－－添加起始颜色
	myGradient.addColorStop(1, "#636363");
	－－添加终止的颜色

	－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

	＃阴影
	ctx.shadowOffsetX = 10; // 设置水平位移
	ctx.shadowOffsetY = 10; // 设置垂直位移
	ctx.shadowBlur = 5; // 设置模糊度
	ctx.shadowColor = "rgba(0,0,0,0.5)"; // 设置阴影颜色

一个也不太搭边儿的例子，不管怎么说，也用到了渐变呢，满是星星的星球：
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E6%98%9F%E7%90%83%E9%87%8C%E7%9A%84%E6%98%9F%E6%98%9F.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E6%98%9F%E7%90%83%E9%87%8C%E7%9A%84%E6%98%9F%E6%98%9F.html)
这里面用到了clip（）方法来实现星星，这跟某css属性clip－path比较神似，我以前尝试过：
请戳：[源码](https://github.com/zhangmengxue/Practice/blob/master/clip-path%E7%A5%9E%E6%8A%80%E5%B7%A7.html)   [示例](https://rawgit.com/zhangmengxue/Practice/master/clip-path%E7%A5%9E%E6%8A%80%E5%B7%A7.html)

**2.7变换**

canvas中有我们熟悉的几个变换函数：

	translate(x, y) －－平移
	rotate(angle) －－旋转
	scale(x, y) －－缩放
	transform(a, b, c, d, e, f) －－变换
	上面的变换函数是根据变换矩阵进行的：
	   a   c   e
	［ b   d   f ］
	   0   0   1

简单的小例子：[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/%E6%97%8B%E8%BD%AC%E7%9A%84%E5%B0%8F%E6%96%B9%E5%9D%97.html)   [运行结果](https://rawgit.com/zhangmengxue/Practice/master/canvas/%E6%97%8B%E8%BD%AC%E7%9A%84%E5%B0%8F%E6%96%B9%E5%9D%97.html)

### 三、图像处理

几个关键函数：

	var myImageData = ctx.createImageData(width, height);
	ctx.getImageData(left, top, width, height);//读取canvas的内容，返回一个对象且该对象有一个data属性，可以供我们操作页面的像素。
	ctx.putImageData(myImageData, dx, dy);//将操作好的对象重新绘制在画布中
	ctx.drawImage(img, 0, 0); // 我们常用的drawImage方法，设置对应的图像对象，以及它在画布上的位置

通过getImageData()方法获取到图像对象，访问它的data属性就可以得到一个像素数组，我们对像素进行处理，使我们可以用canvas处理图像。

**3.1 灰度效果**

灰度图（grayscale）就是取红、绿、蓝三个像素值的算术平均值，这实际上将图像转成了黑白形式。假定d[i]是像素数组中一个象素的红色值，则d[i+1]为绿色值，d[i+2]为蓝色值，d[i+3]就是alpha通道值。转成灰度的算法，就是将红、绿、蓝三个值相加后除以3，再将结果写回数组。

	 for(var i=0;i<data.length;i+=4){
         var avg = (data[i]+data[i+1]+data[i+2]) / 3;
         data[i] = avg; //r
         data[i+1] = avg; //g
         data[i+2] = avg; //b
     }

**3.2 复古效果**

复古效果（sepia）则是将红、绿、蓝三个像素，分别取这三个值的某种加权平均值，使得图像有一种古旧的效果。

	for (var i = 0; i < d.length; i += 4) {
		var r = d[i];
		var g = d[i + 1];
		var b = d[i + 2];
		d[i]     = (r * 0.393)+(g * 0.769)+(b * 0.189); // red
		d[i + 1] = (r * 0.349)+(g * 0.686)+(b * 0.168); // green
		d[i + 2] = (r * 0.272)+(g * 0.534)+(b * 0.131); // blue
	}

**3.3 反转效果**

反转效果（invert）是指图片呈现一种色彩颠倒的效果。算法为红、绿、蓝通道都取各自的相反值（255-原值）

	for (var i = 0; i < d.length; i += 4) {
        d[i] = 255 - d[i];
        d[i+1] = 255 - d[i + 1];
        d[i+2] = 255 - d[i + 2];
     }

我尝试了一下灰度图的效果和反转图的效果： [查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/canvas%E5%9B%BE%E5%83%8F%E6%95%88%E6%9E%9C.html)
注意，在我们自己本地写demo准备运行查看结果的时候，getImageData()方法可能会有问题，比如说报这个错：Failed to execute 'getImageData' on 'CanvasRenderingContext2D': The canvas has been tainted by cross-origin data
我们知道这是域和本地运行等相关的问题，这时我们自己本地启用服务器，localhost访问就可以了。

**3.4 拾色器**

实现思路：通过getImageData(x,y,1,1)获得当前鼠标所在位置的一像素的图像对象，通过其data属性操作拿到它的像素值，也就是我们想得到的rgb值。
[查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/canvas%E6%8B%BE%E8%89%B2%E5%99%A8.html)

**3.5 放大镜**

实际上drawImage方法可以有多个参数，提供更加丰富的功能：

	1  ctx.drawImage(image, dx, dy);
	2  ctx.drawImage(image, dx, dy, dWidth, dHeight);
	3  ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);

各个参数就像下面的示意图这样：

![图3](/img/canvas3.png)

然后我们就可以使用drawImage方法在原图像变换到绘制的图像中做一个等比放大的操作，就可以实现放大镜的效果啦： [查看源码](https://github.com/zhangmengxue/Practice/blob/master/canvas/canvas%E6%94%BE%E5%A4%A7%E9%95%9C%E6%95%88%E6%9E%9C.html)

**3.6图像的高斯模糊**

其实，css中有一个兼容性不那么好的filter属性，可以简单的实现图像模糊的效果：

	 -webkit-filter: blur(20px); /* Chrome, Opera */
	    -moz-filter: blur(20px);
	     -ms-filter: blur(20px);    
	         filter: blur(20px);

但是使用canvas可以实现真正意义上的高斯模糊（就是算法那种balabala的）。
这里有一个实现的很好的高斯模糊的js：[StackBlur](http://www.quasimondo.com/StackBlurForCanvas/StackBlurDemo.html)
使用它我们就可以轻松的实现图像的高斯模糊了：[查看源码](http://www.quasimondo.com/StackBlurForCanvas/StackBlurDemo.html)

### 四、资源

canvas中绘图环境所有的属性和方法都可以在这里找到：[CanvasRenderingContext2D interface](https://developer.mozilla.org/en/docs/Web/API/CanvasRenderingContext2D)

[使用CSS将图片转换成模糊(毛玻璃)效果](http://www.zhangxinxu.com/wordpress/2013/11/%E5%B0%8Ftip-%E4%BD%BF%E7%94%A8css%E5%B0%86%E5%9B%BE%E7%89%87%E8%BD%AC%E6%8D%A2%E6%88%90%E6%A8%A1%E7%B3%8A%E6%AF%9B%E7%8E%BB%E7%92%83%E6%95%88%E6%9E%9C/)

[Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)

[javascript标准参考教程Canvas](http://javascript.ruanyifeng.com/htmlapi/canvas.html)