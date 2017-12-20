CodeboyBlog
---

**[codeboy.me](https://www.codeboy.me)的网站模板**

其中搜索模块可以单独安装，地址如下

- jekyll-search [https://github.com/androiddevelop/jekyll-search](https://github.com/androiddevelop/jekyll-search)
- hexo-search [https://github.com/androiddevelop/hexo-search](https://github.com/androiddevelop/hexo-search)

![网站截图](codeboy.me.png)

本工程实时预览地址 [blogtest.codeboy.me](http://blogtest.codeboy.me)

博客可以简单的搭配微信小程序查看, 具体查看 [https://github.com/androiddevelop/WechatBlog](https://github.com/androiddevelop/WechatBlog)

## 安装方式:

1. 安装jeykll

	```
	gem install jekyll
	gem install jekyll-paginate
	```
2. 将CodeboyBlog复制到服务器(部署到github.io的方式自行搜索)。
3. 运行命令生成网站即可(经常改变配置的话不建议增量更新)。

    ```
    jekyll serve --watch &
    jekyll serve --watch --incremental &  ##增量更新
	```
为了能够更好的生成网站，我们可以写一个脚本:

	```
    #!/bin/bash
    ps aux |grep jekyll |awk '{print $2}' | xargs kill -9
    cd /path/to/blog
    jekyll serve --watch &
    ```

> ps开头的命令是关闭所有jekyll的进程
>
> cd到网站的根目录
>
> 启动jekyll服务

## 需要配置的内容:

1. 修改_config.yml中的信息(知乎等帐号，特别需要注意的是网易云跟贴(cloud_tie_public_key)必须更换，否则您将不能查看到博客的评论)。
2. 修改about/index.html中个人信息(如果不需要个人简介，可以在步骤3中去除对应标签)。
3. 修改_include/nav.html,选择自己需要的导航标签(主页, 应用, 标签, 关于等)
4. 如果博客底部的github，知乎等需要修改，请编辑_includes/footer.html中分享的信息。
5. 去除CNAME文件，或者CNAME文件中的域名更换为您的博客域名。

## 更新内容:

#### 2017-12-19

- 代码显示行号。(/css/syntax.css)

#### 2017-11-17

- 添加初版微信小程序支持，可以便捷创建专属于自己博客的小程序。

#### 2017-04-05

- 调整_config部分配置。
- 由于多说即将关闭，评论组件调整为网易云跟贴。

#### 2017-02-19

- 修正文章双滚动条

### 2016-11-22

- 升级jekyll-search(v1.0.1), 兼容firefox。 (/search/js/cb-search.js)
- 修正移动端宽度展示。(/_layouts/page.html)

### 2016-06-12

- 修正jekyll版本升级至3.1.0+后tags页面的显示问题。
- 将jquery、bootstrap等引用改为cdn，减少站内流量消耗。

### 2015-12-20

- 多说id移动至_config.xml文件中，集中配置。

### 2015-12-09

- 更新_config.yml配置，适配jekyll 3.0+版本。
- 更新博客中相关链接，便于博客转向https站点。
- 去除anchor,标题前面不再显示 `#` 号。

#### 2015-10-10

- 在Clean Blog的基础上修改，同时加入黄玄在CleanBlog上添加的云标签。
- 加入文章搜索功能，pc上可以双击ctrl触发。
- 优化界面，更好的适配手机。


## License

```
Copyright 2016 Yuedong.li

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

> 有任何问题,欢迎发送邮件到app@codeboy.me交流.
