---
layout: post
title: '配置ftp服务器只能上传不能进行其他操作'
date: '2015-01-06'
header-img: "img/post-bg-unix.jpg"
tags:
     - server
     - web
author: 'Codeboy'
---

又到期末考试了，今年当了数据挖掘助教，课程有一道编程大作业，需要搭建ftp服务器，实现文件上传，但是禁止下载重命名。

服务器系统是ubuntu12.04 server，使用的ftp服务器也是linux下大名鼎鼎的vsftpd，配置如下：

**1. 创建用户dm,将其登录终端设置为/bin/false,防止用户ssh登录**

	useradd -m -s /bin/false dm
	
**2. 将/bin/false加入/etc/shells中，使其可以使用dm用户进行ftp登录**

	echo "/bin/bash">>/etc/shells

**3. 配置vsftpd.conf,禁止用户访问上层目录.自行创建/etc/vsftpd.chroot_list,不添加任何用户，在vsftpd.chroot_list中得用户可以切换到上层目录，我们这里需要禁止dm用户。主要配置如下:**

	chroot_local_user=YES
	chroot_list_enable=YES                                                                                                                                       
	chroot_list_file=/etc/vsftpd.chroot_list

**4. 添加相应权限，防止用户下载重命名**

使用cmds_allows命令配置，将不允许的命令(重命名,下载,删除,创建文件夹)除去即可:

	cmds_allowed=FEAT,REST,CWD,LIST,MDTM,NLST,PASS,PASV,PORT,PWD,QUIT,RMD,SIZE,STOR,TYPE,USER,ACCT,APPE,CDUP,HELP,MODE,NOOP,REIN,STAT,STOU,STRU,SYST


主要命令解释如下:

	MKD - make a remote directory 新建文件夹
	NLST - name list of remote directory
	PWD - print working directory 显示当前工作目录
	RETR - retrieve a remote file 下载文件
	STOR - store a file on the remote host 上传文件 
	DELE - delete a remote file 删除文件
	RMD - remove a remote directory 删除目录
	RNFR - rename from 重命名
	RNTO - rename to 重命名
	ABOR - abort a file transfer 取消文件传输
	CWD - change working directory 更改目录
	DELE - delete a remote file 删除文件
	LIST - list remote files 列目录
	MDTM - return the modification time of a file 返回文件的更新时间
	MKD - make a remote directory 新建文件夹
	NLST - name list of remote directory
	PASS - send password
	PASV - enter passive mode 
	PORT - open a data port 打开一个传输端口
	PWD - print working directory 显示当前工作目录
	QUIT - terminate the connection 退出
	RETR - retrieve a remote file 下载文件
	RMD - remove a remote directory
	RNFR - rename from
	RNTO - rename to
	SITE - site-specific commands
	SIZE - return the size of a file 返回文件大小
	STOR - store a file on the remote host 上传文件
	TYPE - set transfer type
	USER - send username


> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
