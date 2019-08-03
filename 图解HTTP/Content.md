# 第 1 章　了解 Web 及网络基础

## 1.1　使用 HTTP 协议访问 Web

很粗糙地介绍了一下输入 URL 如何呈现页面。

Web 是建立在 HTTP （HyperText Transfer Protocol，超文本传输协议）协议上通信的。

## 1.2　HTTP 的诞生

介绍一下 HTTP 诞生的背景。

## 1.3　网络基础 TCP/IP

### 1.3.1　TCP/IP 协议族

**TCP/IP 是互联网相关的各类协议族的总称**

### 1.3.2　TCP/IP 的分层管理

{!!!} TCP/IP 协议族按层次分别分为以下**4 层**：应用层、传输层、网络层和数据链路层。（分层的好处：若是某个地方改变设计，只需把变动的层替换掉即可。设计也变得相对简单。）

**应用层**：决定了向用户提供应用服务时通信的活动。（**对应进程**）（FTP、DNS、HTTP）
**传输层**：提供处于网络连接中的*两台计算机之间*的数据传输。（**对应机器**）（TCP、UDP）
**网络层**：处理在网络上流动的数据包；（多台）在众多的选项内选择一条传输路线。（**找路**）（数据包是网络传输的最小数据单位）（ARP、IP）
**数据链路层**：处理连接网络的硬件部分。包括控制操作系统、硬件的设备驱动、网卡，及光纤等物理可见部分（还包括连接器等一切传输媒介）。（**硬件**）

### 1.3.3　TCP/IP 通信传输流

用 HTTP 举例来说明：
```
客户端：应用层（HTTP） -> 传输层（TCP） -> 网络层（IP） -> 数据链路层（网络）
服务器：逆序
```

![TCP_IP_通信传输流](https://raw.githubusercontent.com/514723273/.md-Pictures/master/TCP_IP_通信传输流.png)


## 1.4　与 HTTP 关系密切的协议 : IP、TCP 和 DNS

### 1.4.1　负责传输的 IP 协议

IP（Internet Protocol）**网际协议**位于网络层。（是协议，不是地址）

IP 协议的作用：是把各种数据包传送给对方。（而要保证确实传送到对方那里，则需要满足各类条件。其中两个重要的条件是 IP 地址和 MAC 地址（Media Access Control Address）。）（IP 地址指明了节点被分配到的地址，MAC 地址是指网卡所属的固定地址。）

**使用 ARP 协议凭借 MAC 地址进行通信**

ARP 协议（Address Resolution Protocol）（地址解析协议）（网络层）

IP 间的通信依赖 MAC 地址。在进行中转时，会利用下一站中转设备的 MAC 地址来搜索下一个中转目标。

### 1.4.2　确保可靠性的 TCP 协议

TCP 协议为了*更容易传送大数据*才把数据分割，而且 TCP 协议能够*确认数据最终是否送达到对方*（三次握手）。

{!!!} 发送端首先发送一个带 SYN 标志的数据包给对方。接收端收到后，回传一个带有 SYN/ACK 标志的数据包以示传达确认信息。最后，发送端再回传一个带 ACK 标志的数据包，代表“握手”结束。（标准答案，背）（synchronize、acknowledgement）

## 1.5　负责域名解析的 DNS 服务

DNS（Domain Name System）（域名系统）

提供域名到 IP 地址之间的解析服务。

## 1.6 各种协议与 HTTP 协议的关系

![各种协议与HTTP协议的关系](https://raw.githubusercontent.com/514723273/.md-Pictures/master/各种协议与HTTP协议的关系.png)

## 1.7 URI 和 URL

- URI（Uniform Resource Identifier）（统一资源*标识*符）
- URL（Uniform Resource Locator）（统一资源*定位*符）

URI 用字符串标识某一互联网资源，而 URL 表示资源的地点（互联网上所处的位置）。URL 是 URI 的子集。

URL 正是使用 Web 浏览器等访问 Web 页面时需要输入的网页地址。

## 1.7.2 URI 格式

![URI格式](https://raw.githubusercontent.com/514723273/.md-Pictures/master/URI格式.png)

