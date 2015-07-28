---
layout: post
title: 'Apache下配置认证用户'
date: '2014-11-16'
header-img: "img/post-bg-unix.jpg"
tags:
     - server
author: 'Codeboy'
---


有时候我们需要给我apache服务器下制定的目录加上用户认证，方便一些而用户进行文件的浏览。配置如下:

1 设置用户
----
	htpasswd -c file_path user_name
	
回车之后输入密码即可，请确保命令中的file _path有其他用户读的权限。

2 设置apache
----
在/etc/apache2/apache2.conf或/etc/httpd/conf/httpd.conf中添加以下内容

	<Directory /var/www/html/picture>
		AuthName "Important Directory"  #登录时提示的内容
		AuthType Basic  #认证方式
		IndexOptions Charset=GB2312  #网页编码
		Options Indexes FollowSymLinks MultiViews #以目录形式展示
		AuthUserFile /opt/.apache_user #用户文件，1中file_path
		Require valid-user 
	</Directory>

若要隐藏服务器标示，请在配置文件中加入以下信息：

	ServerSignature Off
	ServerTokens Prod

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
