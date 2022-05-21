---
title: "配置"
date: 2022-05-17T23:39:55+08:00
draft: false
weight: 2
---

这篇文字主要讲了两层次的配置，包括一个必须的本地配置和一个可选的路由器配置，如果你只是单机使用，则无需配置路由器。
{{< toc >}}

## 本地配置
使用坦克DNS是必须要进行本地配置的，需要配置DNS服务器地址，配置过程很简单。   
### 回环地址
这些视频会使用一个叫回环地址的IP，即`127.0.0.1`这个IP，这个IP就是电脑的自身地址，就像你叫“爸”代表的是你爸，你同学叫“爸”代表你同学的爸，你通过`127.0.0.1`访问你的电脑，你同学通过`127.0.0.1`访问的是他的电脑一样。你不能通过回环地址访问到你同学的电脑，你叫“爸”，你同学的爸不会答应。

### 配置视频
{{< expand "Windows 10" >}}
{{< video src="/video/TankDNS.configuration.window.mp4" type="video/mp4" >}}
{{< /expand >}}

{{< expand "CentOS Stream 9（Ubuntu desktop 22类似）">}}
{{< video src="/video/TankDNS.configuration.centos.mp4" type="video/mp4" >}}

{{< hint type="note" title="注意" >}}
Ubuntu需要关闭自带的域名解析器，使用下列命令
`sudo systemctl disable systemd-resolved.service`
然后**重启电脑**。
如果要重新打开自带的域名解析器，将上面的命令`disable`改为`enable`后执行一次，重启电脑即可。
{{< /hint >}}
{{< /expand >}}

{{< expand "深度桌面 20">}}
{{< video src="/video/TankDNS.configuration.deepin.mp4" type="video/mp4" >}}
{{< /expand >}}

{{< expand "优麒麟 22">}}
{{< video src="/video/TankDNS.configuration.ubuntukylin.mp4" type="video/mp4" >}}

{{< hint type="note" title="注意" >}}
优麒麟需要关闭自带的域名解析器，使用下列命令
`sudo systemctl disable systemd-resolved.service`
然后**重启电脑**。
如果要重新打开自带的域名解析器，将上面的命令`disable`改为`enable`后执行一次，重启电脑即可。
{{< /hint >}}
{{< /expand >}}

## 路由器配置（可选）
如果你是在一个团队里面开发，就会有不同的开发者，不同的需求。他们想访问您的Web应用看看进度，或者测试应用，哪是不是也要安装TankDNS，答案是不用。   
通过路由器配置，使网络内的电脑通过DHCP客户端自动获得DNS服务器地址，每个路由器都包含一个DHCP服务器，每台电脑都自带有一个DHCP客户端并且自动运行，无需我们参与。我们只需要配置路由器DHCP服务器就可以了。

### 故事
昨天，我给我的电脑安装了一个DNS服务和一个Web服务，配置的时候使用还好好的，一切正常，然后我高兴地出去喝奶茶了，由于没钱，我跟老板赊帐，还勉强点了一杯便宜的。   
到了今天，我打开电脑，昨天安装的服务今天一个也访问不了，我找了各种资料，也询问了AI，最终发现原来电脑IP变了，由于DNS服务器IP地址设定了`192.168.1.109`今天打开电脑的时候电脑IP变成了`192.168.1.110`，为了解决这个问题，我把DNS服务器地址改成了`192.168.1.110`，这些服务又正常了。这个问题我搞了一天时间才发现。心里想，我是多么的无能，没脸面对世人。

### 静态IP
在配置路由器DHCP服务器时，先让我给您普及一下**静态IP**与**动态IP**（对这个概念熟悉的用户可以跳过）。这两种IP的最大不同是可变性，当您电脑插入网线连接路由器时，路由器会分配给你一个IP地址，当你重启电脑的时候，这个IP地址默认是会变化的，第一次分配给你的IP地址在你关机的时候可能分给了刚刚连接你路由器上的手机了，于是路由器会分给你一个新的IP。   

静态IP是不会变化的，不管你电脑关机后再开机，都会给你一个相同的IP。即使你电脑关机了有手机连接进入路由器，路由器也不会把这个IP分配给它。静态IP的原理是把IP与电脑MAC绑定。路由器在分配IP的时候会查询MAC地址，然后分配IP。

### 为电脑分配静态IP
静态IP不会对电脑产生任何影响，有些功能必须依赖静态IP来实现，比如DNS，Web服务等等。
{{< expand "TP-LINK路由器配置静态IP">}}
这个视频是在Centos stream 9上录制的，先在设置里面获取本机电脑的MAC地址，然后使用浏览器打开TP-LINK路由器的管理界面，找到应用管理-IP与MAC绑定，然后在表格里找到对应的MAC地址，在这个行点击绑定，最后查看绑定是否成功。本视频最后配置的静态IP是`192.168.1.110`。
{{< video src="/video/TankDNS.configuration.static.ip.mp4" type="video/mp4" >}}
{{< /expand >}}

### 配置DHCP服务器
最后，我们配置DHCP服务器，只需要配置DHCP服务器的DNS服务器部分即可。
{{< expand "TP-LINK路由器配置DHCP">}}
这个视频，我们把刚刚配置的静态IP写入首选DNS服务器即可，备用DNS服务器你想写那个就那个，前提是可以用。
{{< video src="/video/TankDNS.configuration.dns.ip.mp4" type="video/mp4" >}}
{{< /expand >}}

## 总结
使用TankDNS或者其他DNS，配置部分是避免不了的，我觉得使用坦克DNS替代hosts文件是一种好的方法，虽然hosts文件学习成本底，也不用配置，但是坦克DNS只需要配置一次就可以了，功能比hosts多，这也不能怪hosts，因为各有各的目的。   
本地配置是必须的，而路由器配置是可选的。