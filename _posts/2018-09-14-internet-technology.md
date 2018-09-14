---
layout:       article
title:        互联网技术 个人笔记
key:          2018-09-14
tags:         internet
categories:   notes
date:         2018-09-14 10:06:13
modify_date:  2018-09-14 11:28:16
---

互联网技术 知识碎片整理

<!--more-->

## 杂项

### PAP 和 CHAP 的区别

区别在于认证过程，PAP是简单认证,明文传送,客户端直接发送包含用户名/口令的认证请求,服务器端处理并回应。而CHAP是加密认证,先由服务器端给客户端发送一个随机码challenge,客户端根据challenge对口令进行加密,算法是md5(password,challenge,ppp_id).然后把这个结果发送给服务器端.服务器端从数据库中取出口令password2,同样进行加密处理。最后比较加密的结果是否相同.如相同,则认证通过,向客户端发送认可消息

**PAP 和 CHAP 的意思**

GPRS设置中的PAP鉴权和CHAP鉴权有何区别分类:PAP和CHAP是目前的在PPP中普遍使用的认证协议。
PAP是简单二次握手身份验证协议,用户名和密码明文传送,安全性低.PAP全称为：Password Authentication Protocol。
CHAP是一种挑战响应式协议,三次握手身份验证,口令信息加密传送,安全性高. CHAP全称为：Challenge Handshake Authentication Protocol。

### NAT ALG

NAT ALG（Application Layer Gateways）：NAT 应用层网关。

> 使 DNS，FTP，H323，SIP，HTTP，ILS，MSN/QQ，NBT，RTSP，PPTP，TFTP等协议在 NAT 网络环境中正常使用。

ALG涉及到的两个概念：

会话：记录了传输层报文之间的交互信息，包括源IP地址、源端口、目的IP地址、目的端口，协议类型和源/目的IP地址所属的VPN实例。交互信息相同的报文属于一条流，通常情况下，每个会话对应出方向和入方向的两条流。

动态通道：当应用层协议报文中携带地址信息时，这些地址信息会被用于建立动态通道，后续符合该地址信息的连接将使用已经建立的动态通道来传输数据。

https://blog.csdn.net/Fly_as_tadpole/article/details/80645576

### 8021q、8021p

802.1P 是 IEEE 802.1Q （VLAN 标签技术）标准的扩充协议，它们协同工作。IEEE 802.1Q 标准定义了为以太网 MAC 帧添加的标签。VLAN 标签有两部分：VLAN ID （12比特）和优先级（3比特）。 IEEE 802.1Q VLAN 标准中没有定义和使用优先级字段，而 802.1P 中则定义了该字段。

[8021P 百度词条](https://baike.baidu.com/item/IEEE%20802.1P/3003860)。对应配置：在 domain 视图下`trust 8021p`

### Route Distinguisher

路由区分符(Route Distinguisher：RD)和路由目标(Route Target：RT)

**RD**：VPN中IP地址的规划是由客户自行制订的，因而有可能会出现客户选择在RFC1918中定义的私有地址作为他们的站点地址或者不同的VPN使用相同的地址域，也就是所谓的地址重叠现象。地址重叠的后果之一就是BGP无法区分
来自不同VPN的重叠路由，从而导致某个站点不可达。为了解决这个问题，BGP/MPLS VPN除了采用在PE路由器上使用多个VRF表（Virtual Routing Forwarding VPN路由转发表）的方法，还引入了RD的概念。RD具有全局唯一性，通过将8个字节的RD作为IPv4地址前缀的扩展，使不唯一的IPv4地址转化为唯一的VPN-IPv4地址。VPN-IPv4地址对客户端设备来说是不可见的，它只用于骨干网络上路由信息的分发。RD和VRF表之间建立了一一对应的关系。通常情况下，对于不同PE路由器上属于同一个VPN的子接口，为其所对应的VRF表分配相同的RD，换句话说，就是为每一个VPN分配一个唯一的RD。但是对于重叠VPN，即某个站点属于多个VPN的情况，由于PE路由器上的某个子接口属于多个VPN，此时，该子接口所对应的VRF表只能被分配一个RD，从而多个VPN共享一个RD。

**RT**：RT的作用类似于BGP中扩展团体属性，用于路由信息的分发。它分成Import RT和Export RT，分别用于路由信息的导入、导出策略。当从VRF表中导出VPN路由时，要用Export RT对VPN路由进行标记;在往VRF表中导入VPN路由时，只有所带RT标记与VRF表中任意一个Import RT相符的路由才会被导入到VRF表中。RT使得PE路由器只包含和其直接相连的VPN的路由，而不是全网所有VPN的路由，从而节省了PE路由器的资源，提高了网络拓展性。RT具有全局唯一性，并且只能被一个VPN使用。通过对Import RT和Export RT的合理配置，运营商可以构建不同拓扑类型的VPN，如重叠式VPN和Hub-and-spoke VPN。

### import-route unr

UNR（User Network Route）主要用于在用户上线过程中由于无法使用动态路由协议时给用户流量分配路由。BAS里面普通用户通过PPOPE，DHCP获取到的路由，在BAS上都显示为UNR路由。或者说是BAS里面的地址池路由。

### L2TP 协议组网

https://zhidao.baidu.com/question/152442262.html

在使用L2TP协议构建的VPDN典型组网中，包含LAC和LNS两部分。

1、LAC LAC表示L2TP访问集中器（L2TP Access  Concentrator），是附属在交换网络上的具有PPP端系统和L2TP协议处理能力的设备。LAC一般是一个网络接入服务器NAS，主要用于通过PSTN/ISDN网络为用户提供接入服务。LAC位于LNS和远端系统（远地用户和远地分支机构）之间，用于在LNS和远端系统之间传递信息包，把从远端系统收到的信息包按照L2TP协议进行封装并送往LNS，将从LNS收到的信息包进行解封装并送往远端系统。LAC与远端系统之间可以采用本地连接或PPP链路，VPDN应用中通常为PPP链路。

2、LNS LNS表示L2TP网络服务器（L2TP Network Server），是PPP端系统上用于处理L2TP协议服务器端部分的设备。它作为L2TP隧道的另一侧端点，是LAC的对端设备，是被LAC进行隧道传输的PPP会话的逻辑终止端点。

3、说明 用户――（ISDN/PSTN）――LAC――（L2TP协议）――LNS―――企业内部网络

### uRPF

9312 接口下配有 ` ip urpf loose allow-default`。

uRPF（Unicast Reverse Path Forwarding）是一种单播反向路由查找技术，用于防止基于源地址欺骗的网络攻击行为。[百科](https://baike.baidu.com/item/uRPF/6626732)

### LACP

9312 接口下配有 

```
mode lacp-static
lacp timeout slow
```

LACP (Link Aggregation Control Protocol,链路汇聚控制协议)。[百科](https://baike.baidu.com/item/LACP/7797186)