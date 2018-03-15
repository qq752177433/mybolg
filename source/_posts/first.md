---
title: 我的hexo搭建（基于VPS）
date: 2018-03-05 16:19:33
tags: hexo
categories: 环境搭建
toc: true
---
### 前言
  作为一名程序猿，有一个自己的blog还是很有必要的，之前因为种种原因，一直没有时间去做自己的blog，之前看到有同学用的这个hexo搭建的blog,样式美观结构清晰，打算自己也花点时间搞一搞。这里分享一下我的心得。
  <!-- more -->
  **文档参考：** [hexo官网](https://hexo.io/) [hexo搭建方案](http://byxs0x0.top/2018/02/03/Hexo-Install/)
### 环境需求
服务器操作系统：centos6,本地操作系统：windows10,Node环境,Git环境
### 本地配置
1. windows环境下可以直接下载安装包下载Nodejs和Git环境:[Nodejs安装](http://nodejs.cn/download/) [Git安装](https://git-scm.com/download/win)
2. git的初次使用需要进行一些配置 [详情请点击我](http://www.cnblogs.com/superGG1990/p/6844952.html)
3. 上面步骤会得到文件:
{% asset_img t1.png t1 %}
上面的id_rsa.pub等会儿要用与做免密登录用。
4. 在本地安装hexo：
{% codeblock %}
#全局安装hexo客户端
npm install -g hexo-cli
#创建一个存放hexo文件的文件夹比如在e盘下的hexo文件夹，创建完成后进入
cd /hexo
#Hexo会在当前目录初始化
hexo init
#再安装两个Hexo的必要插件
npm install hexo-deployer-git --save
npm install hexo-server
{% endcodeblock %}
5.打开hexo目录下的 _config.yml文件
{% asset_img t4.png t4 %}
把上面的repo中的地址改成服务器的地址后面是项目地址，稍后配置。**注意这里key的冒号后面必须留一个空格，否则不能运行**
这个作用就是通过**hexo-deployer-git**把项目提交到服务器上的git库，再由**git-hooks**自动将库映射到网站的根目录。
6. 主题修改，可以在[hexo-theme](https://hexo.io/themes/)里面找到自己喜欢的主题，去对应的github里面拷贝下来就好了，具体操作流程[请点击我](http://blog.csdn.net/zhou906767220/article/details/62469909)
### 服务器配置
1.我的环境是CentOS6，安装git，和对应设置。
```
yum install git  # 安装git
curl -sL https://rpm.nodesource.com/setup | bash - # 安装node.js
yum install -y gcc-c++ make # 安装node.js
```
遇到如果commond not find之类的错误下载对应的组件即可，比如缺少vim 就yum install vim。
2.git方面的设置
```
#创建git用户
adduser git
#设置密码
passwd git

#设置公钥
cd /home/git/
mkdir .ssh
vim .ssh/authorized_keys  #复制自己GIT公钥进入,之前的.pub文件的代码复制进来即可

#初始化git仓库，用来和本机的repo做对应
cd /var
mkdir git
cd git
git init --bare blog.git #创建一个裸库

#设置git-hook
vi blog.git/hooks/post-receive  #输入下面两行，然后参数换成自己的

#!/bin/sh
git --work-tree=/var/blog --git-dir=/var/git/blog.git checkout -f

#权限设置
chmod +x blog.git/hooks/post-receive
chown -R git:git /var/git

vi /etc/passwd # 让git用户不能shell登录，如果不需要可以略过
/bin/bash # 将git用户的/bin/bash替换为下面字符串
/usr/bin/git-shell
```
2.站点设置
```
#创建站点根目录，用于发布站点
mkdir /var/blog
#设置权限
chmod 777 /var/blog
```
3.安装nginx用于发布网站以及相关设置。
```
#引用别人的过程
#安装 nginx
#第一步，在/etc/yum.repos.d/目录下创建一个源配置文件nginx.repo：

cd /etc/yum.repos.d/

vim nginx.repo

#填写如下内容：
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1

#保存，则会产生一个/etc/yum.repos.d/nginx.repo文件。

#下面直接执行如下指令即可自动安装好Nginx：

yum install nginx -y


#配置 nginx
#/etc/nginx/nginx.conf文件中http节点（注意是http节点）下，增加以下代码
	server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /var/blog;
        index index.html

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

#重启
service nginx restart
```
以上服务器部署完成，应该可以正常访问站点（直接输入ip地址），但是现在还没有上传文章，应该会404
### 操作方法
1. 在本地的hexo目录下右键选择如图按钮
{% asset_img t2.png  t2 %}
2. 输入 hexo new 你的文章名字
{% asset_img t3.png  t3 %}
完成操作后你就可以编辑自己的文章了，在source目录下的 _post下可以找的自己的.md后缀的文件，就可以进行自己编辑了。
3. 编辑完后如果想上传可以输入 hexo clean && hexo d  更多命令参考hexo官网上的文档，在此不过多详述
4. 接下来输入你服务器的ip地址就可以访问了，如果该ip做了域名解析则直接访问域名即可
### 常见问题总结
1. 遇到一些小坑，花了一天总算搭好了自己的博客，参考我朋友 [Hexo搭建遇到的问题](http://www.fclee.xyz/2018/02/06/%E7%AC%AC%E4%B8%80%E6%AC%A1%E4%BD%BF%E7%94%A8hexo/)。
2. 关于不会markdown 参考 [Markdown语法说明](https://www.appinn.com/markdown/) 关于不会hexo的具体操作 参考[hexo官方操作文档](https://hexo.io/zh-cn/docs/).
3. 推荐使用的编辑器atom,配合以下必要插件编辑md文件更加方便
1. **atom-simplified-chinese-menu**  中文包，理解不了转换下看看
2. **language-markdown**  代码高亮
3. **markdown-preview-enhanced**  可视化+同时滚动
4. **markdown-table-editor**  表格
5. **markdown-image-paste**  贴图，记得把_config.yml中的post_asset_folder设置为true，new文章会自动创建一个文件夹
