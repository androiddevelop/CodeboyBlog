---
layout: post
title: 'Android Handler如何避免内存泄露'
date: '2015-06-28'
author: 'Codeboy'
header-img: 'img/post-bg-android.jpg'
tags:
     - android
---
在使用Android Handler的时候，可能会遇到编译器给我们这样的警告:
	
	This Handler class should be static or leaks might occur

造成这个警告的原因是handler持有外层类(Activity等)的引用,同时消息队列中的Message对handler也持有引用，这样就造成一些资源不能回，从而可能造成内存泄露。

解决这个警告的办法即让handler不持有外部类的引用，怎么做到这一点呢，即将Handler设置为静态内部类就行了，将外部类(Activity等)传递给Handler，在Handler中建立弱引用(WeakReference)。

为了能够使以后更好的使用Handler-Message机制，我这里对其进行了封装，使用方法见下。先看一下代码。 

### CBHandler.java	
	package me.codeboy.android.common.component;
	
	import android.os.Handler;
	import android.os.Message;
	
	import java.lang.ref.WeakReference;
	
	import me.codeboy.android.common.app.CBActivity;
	
	/**
	 * Created by yuedong.lyd on 6/7/15.
	 * <p>;
	 *    构建防止内存泄露的handler
	 * </p>
	 */
	public class CBHandler {
	    /**
	     * 防止handler对activity有隐式引用，匿名内部类不会对外部类有引用
	     */
	   public static class UnleakHandler extends Handler {
	        private final WeakReference<CBActivity> activity;
	
	        public UnleakHandler(CBActivity activity) {
	            this.activity =  new WeakReference&lt;CBActivity&gt;(activity);
	        }
	
	        @Override
	        public void handleMessage(Message msg) {
	            super.handleMessage(msg);
	            if(activity.get() == null) {
	                return;
	            }
	            activity.get().processMessage(msg);
	        }
	    }
	}


### CBActivity

	package me.codeboy.android.common.app;
	
	import android.app.Activity;
	import android.os.Bundle;
	import android.os.Message;
	
	import me.codeboy.android.common.component.CBHandler;
	
	/**
	 * Created by yuedong.lyd on 6/8/15.
	 */
	public abstract  class CBActivity extends Activity{
	    public CBHandler.UnleakHandler handler ;
	
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        handler = new CBHandler.UnleakHandler(this);
	    }
	
	    /**
	     * 处理消息
	     * @param msg
	     */
	    public abstract void processMessage(Message msg);
	}
	
在使用中，我们只需自己的Activity继承CBActivity即可，在onCreate时自动创建UnleakHandler的实例handler,从CBHandler的代码中我们也可以看出来，UnleakHandler自动将收到的消息交给CBActivity中的processMessage进行处理。我们只需要在发送消息的时候使用handler变量发送即可，处理在processHandler中处理即可。

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
