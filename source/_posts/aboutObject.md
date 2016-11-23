title: 关于对象的一些判断
date: 2015-01-22 23:23:32
tags:
- 技术文
---

我们经常会遇到这样的问题：判断一个对象是什么样的。是空对象（{}）？还是...？

## 第一种情况

这个肯定会：

	if(data != ''){
		dosomething
	}
但不能只会，很多情况下需要判断的不是字符串，对不？那如果变成对象只会判断：

	if(obj != null){
		dosomething
	}
那在好多情况下就又会进坑了...
为什么呢？去这儿看看：[关于x==y的比较行为](http://barretlee.github.io/ST/ES5.1/#sec-11.9.3)

<!-- more --> 
## 第二种情况

$.isPlainObject()

jQ中的这个方法在对象是通过{}或new Object()创建的的时候会返回true。Zepto中
也有这个方法，大体相同。

源码是这样的：

	isPlainObject: function( obj ) {
		// Not plain objects:
		// - Any object or value whose internal [[Class]] property is not "[object Object]"
		// - DOM nodes
		// - window
		if ( jQuery.type( obj ) !== "object" || obj.nodeType || jQuery.isWindow( obj ) ) {
			return false;
		}

		if ( obj.constructor &&
				!hasOwn.call( obj.constructor.prototype, "isPrototypeOf" ) ) {
			return false;
		}

		// If the function hasn't returned already, we're confident that
		// |obj| is a plain object, created by {} or constructed with new Object
		return true;
	}

可以试试了：

	$.isPlainObject({})          // => true
	$.isPlainObject({'a':'1'})         // => true
	$.isPlainObject(new Object())  // => true
	$.isPlainObject(new Date())   // => false
	$.isPlainObject(window)      // => false

## 第三种情况

判断一个对象是否为空对象：

	isEmptyObject: function( obj ) {
		var name;
		for ( name in obj ) {
			return false;
		}
		return true;
	}

不过有时候它也挺坑爹的：

	$.isEmptyObject({})   // => true
	$.isEmptyObject('')   // => true
	$.isEmptyObject(null)   // => true
	$.isEmptyObject(0)   // => true
	$.isEmptyObject(NaN)   // => true
	$.isEmptyObject(undefined)   // => true
	$.isEmptyObject({'a':'1'})   // => false
	$.isEmptyObject('str')   // => false

其实，我们可以巧妙的应用toString()方法来判断数值类型，不管是基本的值类型还是对象：

	Object.prototype.toString.call(2) // "[object Number]"
	Object.prototype.toString.call('') // "[object String]"
	Object.prototype.toString.call(true) // "[object Boolean]"
	Object.prototype.toString.call(undefined) // "[object Undefined]"
	Object.prototype.toString.call(null) // "[object Null]"
	Object.prototype.toString.call(Math) // "[object Math]"
	Object.prototype.toString.call({}) // "[object Object]"
	Object.prototype.toString.call([]) // "[object Array]"

稍微整理一下就可以像我们想要得到的那样来判断数值类型了，特殊的null和undefined也不会落下：

	var type = function (o){
	    var s = Object.prototype.toString.call(o);
	        return s.match(/\[object (.*?)\]/)[1].toLowerCase();
	};

	type({}); // "object"
	type([]); // "array"
	type(5); // "number"
	type(null); // "null"
	type(); // "undefined"
	type(/abcd/); // "regex"
	type(new Date()); // "date"


具体情况还是要具体分析，头脑清晰想清楚再判断好，再去做接下来的事情。少走些弯路。
