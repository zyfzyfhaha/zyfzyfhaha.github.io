---
title: 在VPS上搭建Shadowsocks出国看看^^
date: 2017-04-10 12:27:25
tags: 
 - VPS
 - Shadowsocks

categories: 
 - VPS
 - Shadowsocks

---
# 简单介绍 #
去年京东搞活动大出血买了个799块ac68u，一直在家当作50块的路由器用(¯﹃¯)。前段时间看到公司小哥有VPS，心血来潮心想自己也整个，刚好也能折腾一哈路由器。
## 神马是VPS & Shadowsocks ##
VPS(Virtual Private Server):虚拟专用服务器。简单的说，vps就是放在国外的一台你的私人电脑，永远处于开机状态。而Shadowsocks呢，就相当于是个梯子。梯子的一头，放在国外的VPS上，就是ss的server端；梯子的另一头，放在你本地的终端上，就是ss的client端。两头一连起来，就能出国咯ヾ(o◕∀◕)ﾉヾ。
<!--more-->

# 购买心仪的VPS #
懒得自己去找，根据公司小哥的推荐，去搬瓦工上看了一圈，就先买了一个KVM架构的VPS一个月先试试水，2.88刀，而且支持支付宝付款。
整个购买步骤都是按照→[搬瓦工中文网](http://banwagong.cn/)中的文章来做的，大家可以去网站多逛逛，基本上大多数问题都能在上面得到解答。我买的是下面这款：

![搬瓦工KVM架构VPS](http://ojp72m756.bkt.clouddn.com/kvm.png)

唯一的问题是网站提供的5%优惠码不能用，不知道是不是因为我这款刚上架新款的缘故。怒亏0.9941人民币肉有点儿痛的。。(｡•ˇ‸ˇ•｡)。买好之后按照邮件给的ip和port就能连上你的vps啦。


# 搭建Server端Shadowsocks #
接下来先把国外的梯子架起来。安装和配置的步骤在→[GitHub的源码](https://github.com/shadowsocks/shadowsocks/tree/master)上写得很清楚，按步骤一步步来就可以了：
1. 因为我们的VPS默认os是centos6.8 32位的，所以用以下的命令：
 -  `yum install python-setuptools && easy_install pip`
 -  `pip install git+https://github.com/shadowsocks/shadowsocks.git@master`
这边有一个小坑：因为centos6.8 yum最高支持python2.6，如果要安装2.7的话就需要自己下载源码编译安装。具体坑在哪里有点忘记了，安装过程中会有warning。不要怕，不影响。
2. 安装完成之后，就可以对我们server端的ss进行配置了。
 - 终端里敲命令`vi /etc/shadowsocks.json`
 - 按一下`i`键
 - 复制黏贴↓
```
	{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
	}
```
需要替换的只有两个地方。"server"：替换成刚买的VPS的ip；"password"：替换成你自己的password。
 - 按一下esc键，再按`shift+：`键，再输入wq，最后敲回车。
 - 用命令`ssserver -c /etc/shadowsocks.json -d start`把配置好的服务在vps上启起来。
 - 用命令`ps aux|grep shadowsocks.json`检查一下：
 ![shadowsocks.json](http://ojp72m756.bkt.clouddn.com/shadowsocks.json.png)
哦野，成功。


# 各平台的Shadowsocks Client端配置 #
server端的ss配置完成就是成功一大半，接下去只要在自己终端把梯子的另一头架起来就ok了。
## windows端 ##
家里的台式机是win7 64位的，所以在GitHub上下载对应的版本 →[戳我](https://github.com/shadowsocks/shadowsocks-windows/releases/tag/4.0.1)
下载完成之后，解压zip包，双击`Shadowsocks.exe`文件，任务栏右下角会多出一架小飞机。双击它后出现：
![windows ss 配置](http://ojp72m756.bkt.clouddn.com/windows%20vps%20%E9%85%8D%E7%BD%AE.png)
点击添加按钮，然后回忆一下刚在`shadowsocks.json`中配置过的内容：
 - 服务器地址填写`server`的value
 - 服务器端口填写`server_port`的value
 - 密码填写`password`的value
 - 加密默认和文件中的`aes-256-cfb`保持一致
 - 代理端口填写`local_port`的value
 - 点击确定
 
右下角任务栏右键点击小飞机，确认启用系统代理。迫不及待打开1024试一哈：

![youtube windows](http://ojp72m756.bkt.clouddn.com/youtube.png)

吼吼，大功告成，叼叼叼⁽⁽ଘ( ˊᵕˋ )ଓ⁾⁾* 

## OS X端 & IOS端 ##
其实Client端的配置都是大同小异，附上MAC的客户端下载地址，大伙自己整一哈就可以了。
→[戳我](https://busi.me/archives/173/)
配置之后的效果：
- MAC：
![you2b mac](http://ojp72m756.bkt.clouddn.com/mac%20youtube.png)

- iPhone：
![u2b iPhone](http://ojp72m756.bkt.clouddn.com/u2b%20ip.png)

PS：iPhone的客户端推荐使用`Shadowrocket`，App Store售价18RMB，讲道理跟Surge比良心的（328他说。 (???
PPS：搜Surge的时候差点下错，有兴趣的同学可以尝试一下(¯﹃¯) ↓
![surge](http://ojp72m756.bkt.clouddn.com/surge%20gay.png)

## 路由器端 ##
关于路由器的配置这里简单说下步骤：
1. 首先得有个智能路由器
2. 我的是ac68u，找到对应版本的固件。我把它刷成Merlin 380.65-X7.4 →[如果你也是这个路由器的话固件地址戳我](http://koolshare.cn/forum.php?mod=viewthread&tid=82624&extra=page%3D1%26filter%3Dtypeid%26typeid%3D68)
3. 升级固件
4. 找到Software center Tab，安装shadowsocks插件
5. 填写配置信息，就和之前任何client一样
6. 提交

顺利的话就会发现，只要连到这个路由器wifi的设备，不用安装ss的client都可以顺利出国啦。

# 结束撒花 #
用ss的时候发现看u2b速度有点慢，看720P就卡死了。后来又研究了一哈kcptun，配好之后效果肛肛的，1080P无压力，抽空再写一篇^ ^。
本篇到此为止，有啥问题可以一起讨论，谢谢大家收看。
