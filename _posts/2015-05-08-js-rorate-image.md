---
layout: post
title: ' Js实现旋转的图片'
date: '2015-05-08'
header-img: "img/post-bg-web.jpg"
tags:
     - web
author: 'Codeboy'
---

gif可以实现旋转的图片，但是怎么使用js实现的。自己想了一下，打算实现一下，整体思路也很简单，每隔一段时间，旋转一下图片，看起来就像在一直旋转一样。[实例地址](http://example.codeboy.me/rotate/)

旋转rotate.js的代码如下:

	/**
	 * Created by YD on 5/7/15.
	 * Base on Jquery
	 */
	var ele ;

	//自定义函数
	$.fn.extend({
		rotate: function () {
		ele = this ;
		setInterval('singleRotate()',20);
	   }
	});

	//初始角度
	var degree = 0;

	//单次旋转
	function singleRotate() {
		//一次增加50度
		degree = degree + 50 * Math.PI / 180;
		ele.css("transform","rotate("+degree+"deg)");
	}

代码中只需引用一下js就行了，我将其封装后放在了服务器上，大家可以直接引用

引用前记得引用jquery，最后在自己的代码中调用rotate方法即可：

	$(element).rotate();
	
	

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
