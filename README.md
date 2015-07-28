# CodeboyBlog

[codeboy.me](http://codeboy.me)的网站模板

![网站截图](codeboy.me.png)

### 安装方式:

1. 安装jeykll
2. 将CodeboyBlog复制到服务器(部署到github.io的方式自行搜索)。
3. 运行命令生成网站即可。
    
        jekyll serve --watch &

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
1. 修改_config.xml中的信息。
2. 修改_includes/footer.html中分享的信息。
3. 修改_layouts/page.html与_layouts/post.html中页面统计信息。
4. 修改_layouts/post.html中文章评论信息(更换为自己多说评论插件id)。
5. 修改about/index.html中个人信息。

### 更新内容:

1. 在Clean Blog的基础上修改，同时加入黄玄在CleanBlog上添加的云标签。
2. 加入文章搜索功能，pc上可以双击ctrl触发。
3. 优化界面，更好的适配手机。