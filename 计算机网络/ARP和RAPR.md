# RARP
## 协议说明
RARP：Reverse Address Resolution Protocol，逆地址解析协议。  
允许局域网的物理继器从`网关`服务器的ARP表或缓存上请求IP地址。  

比如局域网中一台主机只知道自己的物理地址而不知道自己的Ip地址，可以通过RAPA协议发出征求自身IP地址的广播请求，由`RARP服务器`负责回答。  

RARP协议广泛应用于无盘工作站引导时获取IP地址。RARP允许局域网的物理机器从`网关服务器ARP表`或者缓存上请求其IP地址。

## 工作过程
1. 主机发送一个RARP广播，在广播包中，声明自己的MAC地址并请求任何收到此请求的RARP服务器分配一个IP地址。
2. 本地网段的RARP服务器收到此请求后，检查RARP列表，查找该MAC地址对应的IP地址。
3. 如果存在，RARP服务器给源主机发送一个相应数据包，并将IP地址提供给对方主机使用。
4. 如果不存在，RARP服务器对此不作任何相应。
5. 源主机收到RARP服务的相应信息，就可以利用IP地址进行通信，如果一直没有收到RARP服务器的响应信息，表示初始化失败.
![](https://img-blog.csdnimg.cn/20190414120849462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW5ncWl1bWluZw==,size_16,color_FFFFFF,t_70)
## 参考链接
1. https://blog.csdn.net/chengqiuming/article/details/89294760'

# ARP
## 1. 简述
ARP 协议全称(Address Resolution Protocol)`地址解析协议`。它通过实现用于IP地址到MAC地址的映射，即询问目标IP对应的MAC地址的一种协议。ARP协议在IPv4中极为重要。
>ARP协议仅用于IPv4协议中，IPv6协议使用的时Neighbor Discovery Protocol，译为`邻居发现协议`，被纳入ICMPv6中。  

简而言之，ARP是一种解决地址问题的协议，它以IP地址为线索。定位下一个应该接收数据分包的主机MAC地址。如果目标主机不再同一个链路上，那么会查找下一跳路由器MAC地址。

## 2. 讨论
假设主机A和主机B位于同一链路，不需要经过路由器转换，主机A向主机B发送一个IP分组。主机A的地址是192.168.1.2 主机B的地址是192.168.1.3，它们不知道对方的MAC地址是啥。  

主机A想要获取主机B的MAC地址，主机A通过`广播`的方式向以太网上的所有主机发送一个`ARP请求包`，这个ARP请求包中包含了主机A想要知道的主机B的IP地址的MAC地址。  

主机A发送的`ARP请求包`会被同一链路上的所有主机、路由器接收并进行解析并检查ARP请求包中的信息，如果ARP请求包中的目标IP地址和自己的相同，就会将自己主机的MAC地址写入响应包返回主机A。  

由此，可以通过ARP从IP地址获取MAC地址，实现同一链路上的通信。  

>如果是不同链路的？  
就需要使用`代理ARP`，通常ARP会被路由器隔离，但是采用代理ARP(ARP Proxy)的路由器可以将ARP请求转发给相邻的网段。使多个网段中的节点像在同一网段内通信。

## 3. ARP缓存
ARP高效运行的关键就是维护每个主机和路由器上的`ARP缓存表`。这个表维护着每个IP到MAC地址的设置关系。通过把第一次ARP获取到的MAC地址作为IP对MAC的映射关系到一个ARP缓存表。每发送一次ARP请求，缓存表中的映射关系都会被清除。  
![](https://s3.ax1x.com/2021/01/09/sMDmrR.png)  

**MAC地址缓存有一定期限，超过这个期限，缓存的内容会被清除**

## 4. ARP 请求响应包 结构
![](https://s3.ax1x.com/2021/01/09/sMDMa6.png)

## 5. ARP 抓包
Mac环境下 WireShark进行抓包。
