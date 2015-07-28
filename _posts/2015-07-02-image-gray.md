---
layout: post
title: '图片灰度化'
date: '2015-07-02'
tags:
     - java
     - discover
author: 'Codeboy'
---
怎么将图片灰度化，看到一个黑白滤镜的实现，黑白滤镜原理十分简单,是根据各种颜色在人眼中的亮度响应将rgb三通道的像素转换成单通道的灰度像素.而对于彩色转灰度,有一个很著名的心理学公式:
	
	Gray = R*0.299 + G*0.587 + B*0.114 

下面看一下具体怎么使用，怎么讲一张彩色照片转变为黑白照片。看一段java代码:

	package me.codeboy.lyd.image;

	import javax.imageio.ImageIO;
	import java.awt.*;
	import java.awt.image.BufferedImage;
	import java.io.File;
	import java.io.IOException;

	/**
	 * Created by yuedong on 7/2/15.
	 */
	public class GrayImage {
	    public static void main(String[] args) throws IOException {
	        File file = new File("src.png");
	        File out = new File("out.png");
	        BufferedImage image = ImageIO.read(file);
	        int width = image.getWidth();
	        int height = image.getHeight();
	        for (int i = 0; i < width; i++) {
	            for (int j = 0; j < height; j++) {
	                int rgb = image.getRGB(i, j);
	                int r = rgb & 0x00ff0000 >> 16;
	                int g = rgb & 0x0000ff00 >> 8;
	                int b = rgb & 0x000000ff;

            //根据公式计算
            int color = (int) (r * 0.299 + g * 0.587 + b * 0.114);
            image.setRGB(i, j, new Color(color, color, color).getRGB());
	        }
	    }
	    ImageIO.write(image, "PNG", out);

	    //rgb相同下产生的图片
	    BufferedImage colorImage = new BufferedImage(256, 256 * 3, BufferedImage.TYPE_3BYTE_BGR);
	    for (int i = 0; i < 256; i++) {
	        for (int j = 0; j < 256 * 3; j++) {
	            int k = j/3;
	            colorImage.setRGB(i,j,new Color(k,k,k).getRGB());
	        }
	    }

        File colorFile = new File("color.png");
        ImageIO.write(colorImage, "PNG", colorFile);
      }
	}

代码进行了2个操作，一个讲图片进行灰度化，怎么进行灰度化呢，可以看出，仅仅是获取每一个点的rgb值，按照公式计算出灰度值，然后设置新的rgb值，每一个r,g,b的值都是这个灰度值。处理前后的照片如下:
![img](/img/image-src.png)
![img](/img/image-gray.png)

为什么呢？因为r=g=b时，获取的颜色的区间就是由黑到白，紧接着的代码就是将r=g=b的颜色绘制出来，图片如下(最上面的是r=g=b=0,最下面的是r=g=b=255):
![img](/img/image-rgb.png)


> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
