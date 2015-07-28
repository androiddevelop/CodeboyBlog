---
layout: post
title: 'Android WebView页面加载优化'
date: '2015-07-17'
header-img: "img/post-bg-android.jpg"
tags:
     - android
     - network
author: 'Codeboy'
---

目前webapp越来越多，体验也越来越好，为了能够更好的使用WebView展示出流畅的的页面，可以从以下几点做优化：

- **WebView缓存**
- **资源文件本地存储**
- **减少耗时操作**
- **客户端UI优化**

可能有人会说了，为什么不做成native的呢，这样就不用那么的麻烦了。如果我需要加载的内容都是静态的，当然做成native的是最好的，为什么我们要使用WebView呢，因为它可以加载一些容易改变的内容，同时也方便制作多平台应用。

WebView可以优化的哪些地方呢?  

### WebView缓存

开启WebView的缓存功能可以减少对服务器资源的请求，一般使用默认缓存策略就可以了。

    //设置 缓存模式 
	webView.getSettings().setCacheMode(WebSettings.LOAD_DEFAULT);  
    // 开启 DOM storage API 功能 
    webView.getSettings().setDomStorageEnabled(true); 

### 资源文件本地存储

资源等文件(不需要更新)本地存储，在需要的时候直接从本地获取。哪些资源需要我们去存储在本地呢，当然是一些不会被更新的资源，例如图片文件，js文件，css文件，替换的方法也很简单，重写WebView的方法即可。

	{
	      try {
		      if (url.endsWith("icon.png")) {
		          InputStream is = appRm.getInputStream(R.drawable.icon);
		          WebResourceResponse response = new WebResourceResponse("image/png",
		            "utf-8", is);
		          return response;
		      } else if (url.endsWith("jquery.min.js")) {
		          InputStream is = appRm.getInputStream(R.raw.jquery_min_js);
		          WebResourceResponse response = new WebResourceResponse("text/javascript",
		            "utf-8", is);
		          return response;
		      }
		      } catch (IOException e) {
		     		 e.printStackTrace();
		      }
		      return super.shouldInterceptRequest(view, url);
    }


> 1. appRm为app资源管理器，读取drawable，assets，raw下的资源，都是android系统的一些很简单的函数调用。
> 
> 2. getInputStream的参数代表资源具体位置
>
> 3. WebResourceResponse后的资源类型需要写正确

有些时候我们会为我们的网站加入一些统计代码，这些也可以精简掉(自己使用的CNZZ的大概占的有10k左右)，可以使用Charles对客户端进行抓包查看。

### 减少耗时操作

准确的说，是减少同步操作的操作时间，尽量使用异步操作替代同步操作。如果服务端存在读取数据库和计算耗时的操作，尽量使用异步(ajax)进行操作，把原本的时间花在异步操作上。

举个例子，A页面到B页面，A页面实现登录功能，B页面展示主功能页面，如果让B页面去进行用户登录信息验证的话，B页面加载时间会加长(数据库查询等操作)，同时客户端可能需要提供一个等待框(或进度条等)给用户，那看看在A页面使用异步操作的优势吧：

- 可以提供统一的js等待框，多平台保持一致性，减少客户端代码工作量。
- 加载页面的时间变短。B页面由于减少了耗时的操作，加载时间变短，用户等待时间也变短。
- 可以方便加入一些验证后的控制逻辑，不需要进行页面跳转。A页面可以根据异步操作进行结果判断，做出相应的处理。


### 客户端UI优化

怎么让用户看不到WebView加载前的白色页面呢？首次加载后页面的跳转可以用上面的步骤进行优化，可以提供给用户一个很好的体验，那加载的第一页呢？我们需要WebView预加载页面，这个该怎么做到的呢？下面提供两种方法:

- ViewPager，将欢迎页面与WebView页面一起放进ViewPager中，设置预加载页面个数，使WebView所在页面可以预加载，在加载完毕的时候切换到WebView所在页面。
- FrameLayout，将欢迎页面与WebView页面的布局合在一起，显示在一个页面内，起始隐藏WebView布局，待WebView加载完毕，隐藏欢迎布局，显示WebView布局。

使用FrameLayout简单一些，两种方法都是需要对WebChromeClient的onProgressChanged进行监听，加载完毕进行页面切换，如下：
      
      webView.setWebChromeClient(new WebChromeClient() {
            @Override
            public void onProgressChanged(WebView view, int newProgress) {
                super.onProgressChanged(view, newProgress);
                if (newProgress >= 100) {
                    // 切换页面
                }
            }
        });


经过以上几步的优化，一个流畅的webapp生成了。


> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
