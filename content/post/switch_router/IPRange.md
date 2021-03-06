---
title: "IP地址的计算"
date: 2019-09-04T05:49:23+08:00
draft: false
comment: true
description: "IP地址的计算"
tags:
    - 路由&交换
categories:
    - 路由&交换
---

## 1. 什么是IP地址？(IPv4)

> 所谓 IP 地址就是给每个连接在互联网上的主机分配的一个 32 位地址。

> IP 地址就好像电话号码（地址码）：有了某人的电话号码，你就能与他通话了。同样，有了某台主机的 IP 地址，你就能与这台主机通信了。

> 按照 TCP/IP（Transport Control Protocol/Internet Protocol，传输控制协议/Internet协议）协议规定，IP 地址用二进制来表示，每个 IP 地址长 32bit，比特换算成字节，就是 4 个字节。例如一个采用二进制形式的 IP 地址是一串很长的数字，人们处理起来也太费劲了。为了方便人们的使用，IP 地址经常被写成十进制的形式，中间使用符号「.」分开不同的字节。于是，上面的 IP 地址可以表示为「10.0.0.1」。IP 地址的这种表示法叫做「点分十进制表示法」，这显然比 1 和 0 容易记忆得多。

> **百度百科：[IP→地址](https://baike.baidu.com/item/IP/224599#3)**

## 2. 子网掩码

### 简介

> 子网掩码是在 IPv4 地址资源紧缺的背景下为了解决 lP 地址分配而产生的虚拟 lP 技术。

> **百度百科：[子网掩码→简介](https://baike.baidu.com/item/子网掩码#1)**

### 功能

> 子网掩码是一个 32 位地址，是与 IP 地址结合使用的一种技术。它的主要作用有两个，***一是*** **用于屏蔽 IP 地址的一部分以区别网络标识和主机标识，并说明该 IP 地址是在局域网上，还是在远程网上。** ***二是*** **用于将一个大的 IP 网络划分为若干小的子网络。**

> 使用子网是为了减少 IP 的浪费。因为随着互联网的发展，越来越多的网络产生，有的网络多则几百台，有的只有区区几台，这样就浪费了很多 IP 地址，所以要划分子网。使用子网可以提高网络应用的效率。

> 通过计算机的子网掩码判断两台计算机是否属于同一网段的方法是，将计算机十进制的 IP 地址和子网掩码转换为二进制的形式，然后进行二进制「与」(AND)计算（全 1 则得 1，不全 1 则得 0），如果得出的结果是相同的，那么这两台计算机就属于同一网段。

> #### **声明网络地址与主机地址**

> 表 1 默认子网掩码
> |类别|子网掩码的二进制数值|子网掩码的十进制数值|
> |-|-|-|
> |A|11111111 00000000 00000000 00000000|255.0.0.0|
> |B|11111111 11111111 00000000 00000000|255.255.0.0|
> |C|11111111 11111111 11111111 00000000|255.255.255.0|

> 子网掩码一定是配合 IP 地址来使用的。对于常用网络 A、 B、C 类IP地址其默认子网掩码的二进制与十进制对应关系如表 1 所示。子网掩码工作过程是：将 32 位的子网掩码与 IP 地址进行二进制形式的按位逻辑「与」运算得到的便是网络地址，将子网掩码二进制按位取反，然后 IP 地址进行二进制的逻辑「与」（AND）运算，得到的就是主机地址。如：192.168.10.10 AND 255.255.255.0，结果为 192.168.10.0，其表达的含义为：该 IP 地址属于 192.168.10.0 这个网络，其主机号为 10，即这个网络中编号为 10 的主机。

> #### **划分子网**

> 子网掩码机制提供了子网划分的方法。其作用是：减少网络上的通信量；节省 IP 地址；便于管理；解决物理网络本身的某些问题。使用子网掩码划分子网后，子网内可以通信，跨子网不能通信，子网间通信应该使用路由器，并正确配置静态路由信息。划分子网，就应遵循子网划分结构的规则。就是用连续的 1 在IP地址中增加表示网络地址，同时减少表示主机地址的位数。例如，IP 地址为 130.39.37.100，网络地址为 130.39.0.0、子网地址为 130.39.37.0、子网掩码为 255.255.255.0，网络地址部分和子网标识部分为「1」所对应，主机标识部分为「0」所对应。 使用 CIDR 表示为：130.39.37.100/24 即 IP 地址/掩码长度。其中第三个字节上的 255 所对应的 8 位二进制数值就是将主机地址位数借给了网络地址部分，充当了划分子网的位数。

> **百度百科：[子网掩码→子网掩码的功能](https://baike.baidu.com/item/子网掩码#2)**

## 3. 计算 IP 地址

例1：**131.107.31.126/28**

### 计算网络号

/28 表示这个 IP 地址的前 28 位是网络号。

子网掩码写成点分十进制：255.255.255.240

$28÷8=3···4$

前 3 个字节都是网络号的一部分，所以「关键位置」（网络号和主机号交接的字节）是 IP 地址的第 4 个字节，第 4 个字节的 4 位为「关键位置」的网络号，后 4 位为主机号。

**注：「&」之间的「网络号」、「主机号」和「IP 地址」指的都是「关键位置」内。**

**&+**

所以「块大小」(网络号改变的最小单位)为主机号位数 +1 位 $10000_{[2]}$，转换成十进制数：

$$2^{5-1}=2^{4（主机号位数）+1-1}=2^{4（主机号位数）}=16$$

所以「块大小」=$2^{主机号位数}$

**1. 因为主机号全部为 0，所以一个 IP 地址的网络号不会比它本身大。**

**2. 因为「块大小」是网络号改变的最小单位，所以网络号必须是「块大小」的倍数。**

**3. 因为「块大小」是网络号改变的最小单位，所以网络号和 IP 地址的差不会大于「块大小」。**

$126÷16=7···14$

所以网络号为$16×7=112$

**&-**

算出网络号为：131.107.31.112