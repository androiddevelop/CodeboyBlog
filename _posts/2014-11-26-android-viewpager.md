---
layout: post
title: 'ViewPager自适应高度'
date: '2014-11-26'
header-img: "img/post-bg-android.jpg"
tags:
     - android
author: 'Codeboy'
---

在使用ViewPager的时候发现不能自适应高度，可以重写ViewPager的onMeasure来解决，代码如下：

	import android.content.Context;
	import android.support.v4.view.ViewPager;
	import android.util.AttributeSet;
	import android.view.View;

	/**
	 * 自定义viewPager，实现ViewPager自定义高度 
	 * 
	 * @author Yuedong Li
	 * 
	 */
	public class CBViewPager extends ViewPager {

	    public CBViewPager(Context context, AttributeSet attrs) {
	        super(context, attrs);
	    }

	    public CBViewPager(Context context) {
	        super(context);
	    }

	    /**
	     * 将child高度传递给父类构造函数
	     */
	    @Override
	    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {

	        int height = 0;
	        // 下面遍历所有child的高度
	        for (int i = 0; i < getChildCount(); i++) {
	            View child = getChildAt(i);
	            child.measure(widthMeasureSpec,
	                    MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
	            int h = child.getMeasuredHeight();
	            if (h > height) // 采用最大的view的高度。
	                height = h;
	        }

	        heightMeasureSpec = MeasureSpec.makeMeasureSpec(height,
	                MeasureSpec.EXACTLY);

	        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
	    }
	}

直接使用CBViewPager就可以了。

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
