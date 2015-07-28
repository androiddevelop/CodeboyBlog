---
layout: post
title: 'HashMap的小优化'
date: '2015-04-29'
header-img: "img/home-bg.jpg"
tags:
     - java
author: 'Codeboy'
---

HashMap是我们平日内用的非常多的集合框架，网上介绍有很多的实现原理，在存放数据数量已知的情况下，我们可以在构建hashmap的时候指定其容量，减少扩展空间时消耗的时间。下面看一个例子:

	import java.util.HashMap;
	import java.util.Map;

	/**
	 * HashMap测试
	 * 
	 * @author YD
	 *
	 */
	public class MapTest {
	    public static void main(String[] args) {
	        int num = 10000;  //数据容量
	        Map<Integer, String> map1 = new HashMap<Integer, String>();
	        Map<Integer, String> map2 = new HashMap<Integer, String>(num);
	        Map<Integer, String> map3 = new HashMap<Integer, String>(num * 2);

	        long time1 = System.currentTimeMillis();
	        for (int i = 0; i < num; i++)
	            map1.put(i, "haha");
	        long time2 = System.currentTimeMillis();
	        for (int i = 0; i < num; i++)
	            map2.put(i, "haha");
	        long time3 = System.currentTimeMillis();
	        for (int i = 0; i < num; i++)
	            map3.put(i, "haha");
	        long time4 = System.currentTimeMillis();
	        System.out.println("map1 time: " + (time2 - time1) + "ms");
	        System.out.println("map2 time: " + (time3 - time2) + "ms");
	        System.out.println("map3 time: " + (time4 - time3) + "ms");
	    }
	}


在数据量为10000的时候，多次测试，得出的较多的结果是

	map1 time: 14ms
	map2 time: 9ms
	map3 time: 4ms

在数据量为100000的时候，结果如下

	map1 time: 31ms
	map2 time: 16ms
	map3 time: 9ms

如果数据量更大的时候，升值1000000(一百万)时,结果如下

	map1 time: 119ms
	map2 time: 47ms
	map3 time: 59ms

数据量再增加10倍后(一千万)，结果如下

	map1 time: 7718ms
	map2 time: 1035ms
	map3 time: 2156ms

可以看出，当我们预先知道数据量的时候，在构建hashmap的时候指定数据容量，可以在数据量大的时候减少消耗时间。指定数据容量时，不应该过多的分配空间。


> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
