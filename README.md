# CodeboyBlog

[codeboy.me](http://codeboy.me)的网站模板

![网站截图](codeboy.me.png)

### 安装方式:

1. 安装jeykll

	gem install jekyll
	gem install jekyll-paginate

2. 将CodeboyBlog复制到服务器(部署到github.io的方式自行搜索)。
3. 运行命令生成网站即可(经常改变配置的话不建议增量更新)。
    
        jekyll serve --watch &
        jekyll serve --watch --incremental & //增量更新

为了能够更好的生成网站，我们可以写一个脚本:

    #!/bin/bash
    
    ps aux |grep jekyll |awk '{print $2}' | xargs kill -9
    cd /path/to/blog
    jekyll serve --watch &
    
    
> ps开头的命令是关闭所有jekyll的进程
>
> cd到网站的根目录
>
> 启动jekyll服务

## 需要配置的内容:
1. 修改_config.yml中的信息(知乎等帐号，特别需要注意的是多说评论插件的id必须更换，否则您将不能查看到博客的评论)。
2. 修改about/index.html中个人信息(如果不需要个人简介，可以在步骤3中去除对应标签)。
3. 修改_include/nav.html,选择自己需要的导航标签(主页, 应用, 标签, 关于等)
4. 如果博客底部的github，知乎等需要修改，请编辑_includes/footer.html中分享的信息。

### 更新内容:

#### 2015-12-20
1 多说id移动至_config.xml文件中，集中配置

#### 2015-12-09:

1. 更新_config.yml配置，适配jekyll 3.0+版本。
2. 更新博客中相关链接，便于博客转向https站点。
3. 去除anchor,标题前面不再显示 `#` 号

#### 2015-10-10:

1. 在Clean Blog的基础上修改，同时加入黄玄在CleanBlog上添加的云标签。
2. 加入文章搜索功能，pc上可以双击ctrl触发。
3. 优化界面，更好的适配手机。
