---
title: '用VPS快速搭建ShadowSocks翻墙教程'
date: 2018-03-06 14:31:56
tags: vps翻墙
categories: 环境搭建
toc: true
---
### 前言
  翻墙，这是一个程序猿的必备技能。
  **不翻墙能会怎么样？**
  1.npm 安装过慢，如果不用淘宝的npm源的情况下。
  2.浏览一些国外网站很慢，要么就是连不进去。
  3.github有的时候会登不上。
  总之就是很多事情都做不了，作为一个搞IT的，连个墙都翻不过去，就像井底之蛙，看不到外面的世界，那岂不是太不精彩了。而且我发现最近在百度搜索一些翻墙的关键字都搜索不到，应该是被屏蔽了，导致了想翻墙确找不到方法。在朋友和辛苦查询之下，总结了一种方法，大概就是搭建一个外国的vps然后 用ss代理来完成翻墙操作。
  <!-- more -->
  **参考文档：**
  [搬瓦工Bandwagon快速搭建ShadowSocks翻墙教程](http://www.huizhanzhang.com/2017/05/bandwagon-one-key-shadowsocks.html)
  其实教程基本上是按照这个来的，但是在过程中会出现一些问题，导致中途卡顿了一段时间，所以打算自己也写一下经验
  [vps购买网站BandwagonHost](https://www.bwh1.net/)
  搬瓦工BandwagonHost是一家著名的低价VPS提供商，属于IT7旗下公司。早先曾经退出过年付3美元的VPS，当然这已成为历史。目前在售的vps月付不过3美元，年付不过20美元，年付平均到每月仅10元人民币多一点，比购买现成的VPN便宜很多。
### 环境搭建
1.在BandwagonHost注册一个用户，里面内容能填的就填，填不了就随便填一点，关系不大。
{% asset_img t1.png t1 %}
2.注册完登录之后选择一款服务器进行购买，如果仅仅为了翻墙，要求不用太高，买便宜一点的套餐就可以了,**注意看清楚上面的价格和时间的关系，别买亏了**。
{% asset_img t2.png t2 %}
=======
### vps搭建
1.在BandwagonHost注册一个用户，里面内容能填的就填，填不了就随便填一点，关系不大。
{% asset_img t1.png t1 %}
2.注册完登录之后选择一款服务器进行购买，如果仅仅为了翻墙，要求不用太高，买便宜一点的套餐就可以了,**注意看清楚上面的价格和时间的关系，别买亏了**。购买的时候可以输入优惠码**BWH1ZBPVK**会便宜将近百分之6
{% asset_img t2.png t2 %}
3.**接下来的教程和参考文档的教程差不多，我也就先贴写来了，但是我在搭建的过程中还是遇到了点问题，下面会指出。** 下面，我们以openvz架构的19.99美元一年的机房那款vps为例进行演示。
4.根据你的需要选择订购周期，年付是最便宜的，这里选择年付。随后机房只有一个，没得选。直接点击add to cart，加入到购物车。
{% asset_img t3.png t3 %}
5.这里我们可以使用优惠码，目前搬瓦工最大的优惠码是5.1%优惠码，在左下角的promotional code中输入ireallyreadtheterms8即可，我们可以看到价格变为了18.97美元，折合每月仅10元，非常优惠，比购买vpn便宜很多。关于优惠码，如果失效可以[请点击我](http://www.huizhanzhang.com/category/coupons/bandwagonhost-coupons)。
点击 check out 结账
{% asset_img t4.png t4 %}
6.选择支付宝alipay，勾选统一服务条款，提交
{% asset_img t5.png t5 %}
7.再支付宝界面付款完成后跳转回搬瓦工这时候你的后台就多了一个vps,并且回邮件通知你的vps的ip，端口和密码。
8.然后在个人用户界面可以看到如下界面，或者**参照下图**,点击KiviVM Control Panel进入控制面板
{% asset_img t6.png t6 %}
9.在安装系统之前如果机子没有停，那么安装会失败的，所以我们可以看看机子是否是启动的，没有的话可以在**main control**点击**stop**。
10.建议安装**centos6**系统，否则的话可能对shadowsocks功能不支持，我们首先在后台安装centos6系统。点击左侧的install new os，选择centos-6开头的任意系统。例如，这里选择了带bbr加速的centos6 x86 x64系统。勾选底部的 i agree。。。，点击reload。
{% asset_img t8.png t8 %}
11.几分钟后，系统已经安装好，页面会提示你的密码与ssh端口，**注意保存**。虽然我们一键安装shadowsocks是不需要这个的，但是日后如果需要远程连接你的vps做其他事情还是需要的。
{% asset_img t9.png t9 %}
12.这时查看控制面板首页，系统已经换成了centos6.
{% asset_img t10.png t10 %}
13.这步有点小坑，由于部分套餐没有shadowsocks server,当然有的话可以直接在左侧的**kiwiVM Extras**中看见有个**Shadowsocks Server**的选项，点击后直接install就好了。
{% asset_img t11.png t11 %}
如果没有的话就可能会这样
{% asset_img t12.png t12 %}
百度了一下找到了解决方案，其实该模块只是被隐藏了而已，直接输入 *https://kiwivm.64clouds.com/main-exec.php?mode=extras_shadowsocks* 可以直接进入该模块，然后进行下面的操作。
14.出现下面的时候就安装完毕
{% asset_img t13.png t13 %}
15.我们刷新下页面，重新点击左侧的shadowsocks server菜单，页面已经变成了下图。这**三项**是你链接你的vpn的必要内容，并且搬瓦工贴心的在页面下方展示了教程。
{% asset_img t14.png t14 %}
### 本地下载安装shadowsocks
1.Android用户：从Google Play安装：[Shadowsocks-Android](https://play.google.com/store/apps/details?id=com.github.shadowsocks)
2.iOS用户：从App Store安装：[Shadowsocks-iOS](https://itunes.apple.com/us/app/shadowsocks/id665729974?ls=1&mt=8)
3.对于Windows 7或更早版本，请下载: [shadowsocks-win-2.3.zip](https://kiwivm.64clouds.com/dist/shadowsocks-win-2.3.zip)
4.对于Windows 8或更高版本，请下载: [shadowsocks-win-dotnet4.0-2.3](https://kiwivm.64clouds.com/dist/shadowsocks-win-dotnet4.0-2.3.zip)
5.下载后，解压缩.zip并双击启动Shadowsocks.exe
如果您没有看到如下所示的设置窗口，请在右下角托盘找到并双击Shadowsocks托盘图标
6.shadowsocks设置，输入如下所示的设置，IP是你的vpsIP，端口默认443，密码是当前页面显示的密码，加密方式如图。
然后点击确定
{% asset_img t15.png t15 %}
7.您可以右键单击Shadowsocks托盘纸飞机图标，勾线启用系统代理。然后再次右键单击图标并选择系统代理模式 – >全局或者PAC。全局模式代表所有网站都翻墙，pac模式代表只对pac文件中的网站翻墙，推荐pac模式。pac文件在刚才的解压文件夹中，是pac.txt，自己仿照里面的格式可以添加你需要翻墙的网站。
{% asset_img t16.png t16 %}
8.打开浏览器，访问youtobe，能访问就成功了
{% asset_img t17.png t17 %}
9.补充说明，以上内容参考[搬瓦工Bandwagon快速搭建ShadowSocks翻墙教程](http://www.huizhanzhang.com/2017/05/bandwagon-one-key-shadowsocks.html)，最后祝大家能愉快访问墙外世界！
