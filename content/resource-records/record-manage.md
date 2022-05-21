---
title: "资源记录管理"
date: 2022-05-18T16:43:48+08:00
draft: false
weight: 3
---

目前坦克DNS只支持[记录类型]({{< relref "record-type" >}} "TankDNS支持记录类型")中的6种类型的记录，如果您配置了其他不支持的记录类型，软件会无法识别，也就不会运行。现阶段坦克DNS没有功能提示records.json错误出现在哪一行。

{{< toc >}}

## A记录
### 添加A记录
a记录ip地址必须是合法IPv4地址，否则会出错。
```tpl
[
    {
        "域名":"www.b.lo",
        "记录类型":"a",
        "IPV4地址":"192.168.1.115"
    }
]
```
### 测试：
在Linux终端或Windows PowerShell执行：   
`nslookup -query=a www.b.lo 127.0.0.1`   
输出：
```
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
Name:	www.b.lo
Address: 192.168.1.115
```


## NS记录
### 添加NS记录
ns记录值是一个域名而不是ip。
```tpl
[
    {
        "域名":"www.b.lo",
        "记录类型":"ns",
        "域名服务器":"ns1.b.lo"
    }
]
```
### 测试：
在Linux终端或Windows PowerShell执行：   
`nslookup -query=ns www.b.lo 127.0.0.1`   
输出：
```
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
www.b.lo	nameserver = ns1.b.lo.
```


## AAAA记录
### 添加IPv6地址
现阶段坦克DNS支持的IPv6地址表示法为“冒号十六进制”，所以你必须把地址完整填写，并保证输入的是合法的IPv6地址，否则会出错。想了解更多IPv6信息，[点击这里](https://baike.baidu.com/item/IPv6)
```tpl
[
    {
        "域名":"www.b.lo",
        "记录类型":"aaaa",
        "IPV6地址":"0000:0000:0000:0000:0000:0000:0000:0001"
    }
]
```
### 测试：
在Linux终端或Windows PowerShell执行：   
`nslookup -query=aaaa www.b.lo 127.0.0.1`   
输出：
```
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
Name:	www.b.lo
Address: ::1
```
响应的地址是`::1`，这也是一个IPv6地址，叫“0位压缩表示法”它等于`0000:0000:0000:0000:0000:0000:0000:0001`。


## MX记录
### 添加mx记录
邮件交换服务器记录值是域名不是IP，与其它记录类型对比，多了一个优先级字段，值是整数。
```tpl
[
    {
        "域名":"b.lo",
        "记录类型":"mx",
        "优先级": "10",
        "邮件交换服务器":"mx.b.lo"
    }
]
```
### 测试：
在Linux终端或Windows PowerShell执行：   
`nslookup -query=mx b.lo 127.0.0.1`   
输出：
```
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
b.lo	mail exchanger = 10 mx.b.lo.
```


## CNAME记录
### 添加CNAME记录
cname记录是一个很常用的记录类型，我使用它的频率仅次于a记录。如果你有10个域名指向同样的ip，当你修改ip的时候，你需要执行10次修改动作。而如果你使用cname记录，你只需要执行一次修改动作。只需要先把10个域名cname记录解析到`c.b.lo`域名上，然后添加一个a记录`c.b.lo`指向一个ip，以后修改记录只需要修改这个ip即可。
```tpl
[
    {
        "域名":"www.b.lo",
        "记录类型":"cname",
        "别名":"c.b.lo"
    }
]
```
### 测试：
在Linux终端或Windows PowerShell执行：   
`nslookup -query=cname www.b.lo 127.0.0.1`   
输出：
```
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
www.b.lo	canonical name = c.b.lo.
```


## TXT记录
添加TXT记录。文本记录返回的是文本。
```tpl
[
    {
        "域名":"b.lo",
        "记录类型":"txt",
        "文本":"TankDNS is No bad DNS."
    }
]
```
### 测试：
在Linux终端或Windows PowerShell执行：   
`nslookup -query=txt b.lo 127.0.0.1`   
输出：
```
Server:		127.0.0.1
Address:	127.0.0.1#53

Non-authoritative answer:
b.lo	text = "TankDNS is No bad DNS."
```


## 完整代码
```tpl
[
    {
        "域名":"www.b.lo",
        "记录类型":"a",
        "IPV4地址":"192.168.1.115"
    },
    {
        "域名":"www.b.lo",
        "记录类型":"ns",
        "域名服务器":"ns1.b.lo"
    },
    {
        "域名":"www.b.lo",
        "记录类型":"aaaa",
        "IPV6地址":"0000:0000:0000:0000:0000:0000:0000:0001"
    },
    {
        "域名":"b.lo",
        "记录类型":"mx",
        "优先级": "10",
        "邮件交换服务器":"mx.b.lo"
    },
    {
        "域名":"www.b.lo",
        "记录类型":"cname",
        "别名":"c.b.lo"
    },
    {
        "域名":"b.lo",
        "记录类型":"txt",
        "文本":"TankDNS is No bad DNS."
    }
]