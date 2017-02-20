# CodeboyBlog

[codeboy.me](http://codeboy.me)的网站模板，其中搜索模块可以单独安装，地址如下

- jekyll-search [https://github.com/androiddevelop/jekyll-search](https://github.com/androiddevelop/jekyll-search)
- hexo-search [https://github.com/androiddevelop/hexo-search](https://github.com/androiddevelop/hexo-search)

![网站截图](codeboy.me.png)

### 安装方式:

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

### 需要配置的内容:

1. 修改_config.yml中的信息(知乎等帐号，特别需要注意的是多说评论插件的id必须更换，否则您将不能查看到博客的评论)。
2. 修改about/index.html中个人信息(如果不需要个人简介，可以在步骤3中去除对应标签)。
3. 修改_include/nav.html,选择自己需要的导航标签(主页, 应用, 标签, 关于等)
4. 如果博客底部的github，知乎等需要修改，请编辑_includes/footer.html中分享的信息。

### 更新内容:

#### 2017-02-19

- 修正文章双滚动条

#### 2016-11-22

- 升级jekyll-search(v1.0.1), 兼容firefox。 (/search/js/cb-search.js)
- 修正移动端宽度展示。(/_layouts/page.html)

#### 2016-06-12

- 修正jekyll版本升级至3.1.0+后tags页面的显示问题。
- 将jquery、bootstrap等引用改为cdn，减少站内流量消耗。

#### 2015-12-20

- 多说id移动至_config.xml文件中，集中配置。

#### 2015-12-09

- 更新_config.yml配置，适配jekyll 3.0+版本。
- 更新博客中相关链接，便于博客转向https站点。
- 去除anchor,标题前面不再显示 `#` 号。

#### 2015-10-10

- 在Clean Blog的基础上修改，同时加入黄玄在CleanBlog上添加的云标签。
- 加入文章搜索功能，pc上可以双击ctrl触发。
- 优化界面，更好的适配手机。
