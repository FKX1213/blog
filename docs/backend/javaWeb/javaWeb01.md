# 计算机网络基础

:::details 参考资料：

- [JavaWeb 教程](https://www.bilibili.com/video/BV1CL4y1i7qR/)

:::

## 1.走进网络世界

网络的起源可以追溯到20世纪60年代初，当时美国国防部的高级研究计划署（ARPA）在进行一项名为 `ARPANET`
的项目，旨在建立一种去中心化的通讯网络，来实现军事和科研机构之间的信息交流。`ARPANET`
是世界上第一个广域网，于1969年建立了第一个节点，通过分组交换数据的方式实现了信息传输。

随着时间的推移，`ARPANET`不仅连接了更多的节点，还演变成了现代互联网的雏形。在1970年代和1980年代，出现了 `TCP/IP`
协议奠定了互联网的基础，域名系统（`DNS`）的建立也增加了互联网的易用性。

1990年代互联网开始向公众开放，世界各地的人们可以通过浏览器访问网页、发送电子邮件等方式使用互联网。随着技术的不断进步和普及，互联网已经成为现代社会中不可或缺的一部分，极大地改变了人们的生活方式和工作方式。

### 1.1.互联网和网络设备介绍

现如今，互联网上有着数以万计的网站、应用程序、游戏等，网络可以说是我们身边必不可少的东西，截至2023年12月，中国网民规模达10.92亿人，较2022年12月新增网民2480万人，互联网普及率达77.5%，在我们身边几乎每个人都会上网，并且全世界任何一个角落的人都可以访问到互联网上的内容。

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/11/F6hLfJICTbXxYBs.png" alt="互联网" style="display: block;margin: 0 auto;zoom: 100%">

发的主要方向，就是这些互联网上的内容，比如网站之类的，而网站开发又分为前端开发和后端开发，前端开发负责提供网页的外观、用户交互设计等，也就是客户端，而后端则需要为前端提供数据，前端拿到之后在页面中展示。

普通公民是没办法直接接入到互联网的，一般只能通过一些网络运营商（网络服务供应商，ISP）来接入到互联网，一般比较常见的有以下几种方式：

1. 办理运营商的SIM卡套餐，并插入到支持SIM卡的手机或平板设备，这样就能接入到附近的基站上使用5G或4G网络，随时随地只要附近有基站信号都可以使用。
2. 办理运营商的宽带套餐，一般是光纤入户，通过将小区的光纤接入到家里，实现居家上网，这种网络接入方式相比4G/5G网络延迟更低更稳定，缺点就是位置是固定的。

目前我国一共有四大运营商：

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/11/qO8lr7MexmdVDYw.png" alt="四大运营商" style="display: block;margin: 0 auto;zoom: 100%">

家里的宽带上网，运营商会在小区里部署一系列的光纤设备（有些在小区花园，有些在楼道内）。这些设备与互联网相连接，在办理套餐之后，会有专门的工程师为我们接入，这里就不得不提到光猫和路由器了：

* 光猫：

  光纤入户的必备设备，一般办理宽带套餐之后运营商会直接增送一个（不用千万别扔了，退宽带的时候还要还的）由于光纤中数据以光作为媒介进行传输，而我们的电脑、手机只能接收电信号，因此我们需要通过一个设备将不同信号类型进行转换，而光猫就是做这个事情的

* 路由器：

  由于家里有着非常多的设备，它们都需要连接网络，但是光猫提供的有线网口太少，而且不支持无限连接，比如最常见的WiFi信号，这些功能都可以由路由器来提供，它可以同时连接多个网络的设备并将数据准确发送到互联网，不过现在有一部分运营商的光猫自带路由器功能，也支持WiFi信号发射，但是大部分用户还是会选择单独安装一个路由器使用，毕竟更加专业和稳定

所以，在家中使用互联网，架构一般就像下面这样：

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/11/6OUnHuIGXwRezdq.png" alt="家中互联网架构" style="display: block;margin: 0 auto;zoom: 100%">

通过这样层层连接，我们就可以愉快地上网了。

### 1.2.IP和Port

在生活中，每家每户都有一个门牌号，这样当别人来找我们时，就可以根据地址快速找到我们在什么位置（比如送外卖、送快递等）。而在互联网中，如果要访问某个站点或者是某个站点要向我们发送数据，怎么确定是谁呢，所以每一个设备同样有着自己的地址，现在最常用的就是IP地址。

通过WiFi或者网线连接路由器时，会自动获得一个IP地址（通过DHCP协议自动分配），大家可以打开自己的电脑或手机，在设置中查看自己的IP地址：

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/11/ne7kRDSiu38GAbz.png" alt="家中互联网架构" style="display: block;margin: 0 auto;zoom: 100%">

IP地址一共有两种，一种是 `IPv4` 地址，比如：`192.168.XX.XX` 这样的格式，为什么是这样的格式呢，回到计算机底层去一步一步研究，实际上它以二进制形式来表示长这样：

````java
192.168.0.5=11000000.10101000.00000000.00000101
````

可以看到，一个IPv4地址一共有4段，每段8个bit位，一共32位，和基本类型int一致。也就是说，IPv4能够表示的地址范围，以十进制表示就是：0.0.0.0到255.255.255.255，理论上如果全部拿来使用的话，大约有43亿个地址可用。

但是实际上我们能够使用的地址非常有限，国际上根据不同类型的网络用途，对网段进行了划分，主要分为 A、‌B、‌C、‌D、‌E
五类，‌每类地址都有其特定的用途和特点：

* **A类地址**：‌

  以0开头，‌网络地址空间长度为7位，‌主机地址空间长度为24位。‌A类地址的范围是从1.0.0.0到127.255.255.255。‌A类IP地址适用于有大量主机的大型网络，‌每个A类网络的主机地址数多达16,000,000个。‌

* **B类地址：**

  以10开头，‌网络地址空间长度为14位，‌主机地址空间长度为16位。‌B类IP地址的范围是从128.0.0.0到191.255.255.255。‌B类地址适用于一些国际性大公司与政府机构等，‌每个B类网络的主机地址数多达65536个。‌

* **C类地址：**

  ‌以110开头，‌网络地址空间长度为21位，‌主机地址空间长度为8位。‌C类IP地址的范围是从192.0.0.0到223.255.255.255。‌C类IP地址特别适用于一些小公司与普通的研究机构，‌每个C类网络的主机地址数最多为256个。‌

* **D类地址：**

  ‌以1110开头，‌不标识网络，‌用于其他特殊的用途，‌如多目的地址。‌D类IP地址的范围是从224.0.0.0到239.255.255.255。‌

* **E类地址：**

  ‌以11110开头，‌暂时保留用于某些实验和将来使用。‌E类IP地址的范围是从240.0.0.0到255.255.255.255。‌

在我们国家，一般很少会给个人一个可以直接使用的IP地址（也称为**公网IP地址**
，也就是在互联网中的一个独一无二的IP地址）。实际上家里的宽带上网，一般都是一个小区一栋楼或者一整个小区共用一个IP地址去与互联网上的资源交互。而路由器分配给我们的IP地址，实际上是一个局域网IP地址，局域网顾名思义就是一个局部的网络，这个网络是独立的，所有IP地址也仅仅属于这个局域网内部，就像下面这样：

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/11/KkJvu9rqtULFiTE.png" alt="家中互联网架构" style="display: block;margin: 0 auto;zoom: 100%">

还有一种是 `IPv6` 地址，它的优势就在于它大大地扩展了地址的可用空间。不过在现在来看，IPv6普及率还远不及IPv4，很多网站现在还不支持使用IPv6访问，甚至有些宽带师傅安装时根本不给你开通IPv6功能。

电脑上运行着各种各样的软件，这些软件可能会找不同的IP地址进行通信，但是我们只获得了一个IP地址，如果其他设备要给电脑上某个应用发送数据，那怎么辨别呢❓

在网络通信中，每一个计算机都会有一个或多个端口，用于传输数据和与其他计算机进行通信。每个端口都有一个数字来标识，常见的端口号范围是0到65535，其中0到1023是系统保留端口，用于一些常见的服务，比如HTTP服务使用的端口80，FTP服务使用的端口21等，在Linux或MacOS下普通用户无权使用。

通过端口，不同的应用程序可以同时在计算机上运行并与其他设备进行通信。

端口的出现解决了电脑上不同应用的网络通信问题，两台设备之间相互通信：

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/12/9iT81gtDCGLcdEQ.png" alt="家中互联网架构" style="display: block;margin: 0 auto;zoom: 100%">

### 1.3.域名和DNS

每次通过 IP 去访问对应的资源非常麻烦，怎么记得住呢❓

域名就可以解决，域名是代表互联网资源（网站、服务器），它通常由一个由一个顶级域名（比如.com、.org、.net等）和一个二级域名（比如example.com）组成。

当我们访问域名时，会由域名系统（DNS）查询，域名最终会被转换成一个IP地址，从而帮助用户定位到相应的互联网资源。

## 2.网络通信协议

通信协议是指双方实体完成通信或服务所必须遵循的规则和约定。

### 2.1.OSI七层网络模型

<img src="https://oss.itbaima.cn/internal/markdown/2024/07/12/9iT81gtDCGLcdEQ.png" alt="家中互联网架构" style="display: block;margin: 0 auto;zoom: 100%">
