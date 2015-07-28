---
layout: post
title: 'a+=b 等价于 a=a+b ?'
date: '2015-07-10'
header-img: "img/home-bg.jpg"
tags:
     - java
author: 'Codeboy'
---

**a += b**和**a = a + b**完全等价么(java)？可能很多人以为是一样的，其实并非等价的，下面看一下证据吧。

	public class Test {
	    public static void main(String[] args) {
	        int a = 0;
	        float c = 2.0f;
	        a += c;
	        a = a +  c;  //①
	    }
	}

上面的代码有问题么？ 能够通过编译么？ 答案是**否定的**。

	$ javac Test.java
	Test.java:6: error: possible loss of precision
	         a = a +  c;
	               ^
	  required: int
	  found:    float
	1 error

出现的问题是编译错误， 但是**a += c**却不会出现编译错误，能够正常通过编译。

**为什么为这样呢？**

我们将**①**处代码去除后，顺利编译，可以使用jd-gui等工具看一下**a += c**的反编译后的代码：

	public class Test
	{
	  public static void main(String[] paramArrayOfString)
	  {
	    int i = 0;
	    float f = 2.0F;
	    i = (int)(i + f);
	  }
	}

看一下以下这句:

	 i = (int)(i + f);
	 
可以看出**a += c**在编译的时候做了强制类型转换。

	结论: 
	对于 a += c
	如果a的类型可以兼容b，则 (a += c) 等价于 (a = a + c) 
	否则，则会在a与c做完加法后进行强制转换。

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
