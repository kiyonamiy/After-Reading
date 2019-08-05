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

1. **应用层**：决定了向用户提供应用服务时通信的活动。（**对应进程**）（FTP、DNS、HTTP）
2. **传输层**：提供处于网络连接中的*两台计算机之间*的数据传输。（**对应机器**）（TCP、UDP）
3. **网络层**：处理在网络上流动的数据包；（多台）在众多的选项内选择一条传输路线。（**找路**）（数据包是网络传输的最小数据单位）（ARP、IP）
4. **数据链路层**：处理连接网络的硬件部分。包括控制操作系统、硬件的设备驱动、网卡，及光纤等物理可见部分（还包括连接器等一切传输媒介）。（**硬件**）

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

URI 用*字符串*标识某一互联网资源，而 URL 表示资源的地点（互联网上所处的位置）。URL 是 URI 的子集。

URL 正是使用 Web 浏览器等访问 Web 页面时需要输入的网页地址。

## 1.7.2 URI 格式

![URI格式](https://raw.githubusercontent.com/514723273/.md-Pictures/master/URI格式.png)

# 第 2 章　简单的 HTTP 协议

## 2.1　HTTP 协议用于客户端和服务器端之间的通信

## 2.2　通过请求和响应的交换达成通信

HTTP 协议规定，请求从客户端发出，最后服务器端响应该请求并返回。*（换句话说，肯定是先从客户端开始建立通信的，服务器端在没有接收到请求之前不会发送响应。）*

请求报文例子：

![请求报文的构成](https://raw.githubusercontent.com/514723273/.md-Pictures/master/请求报文的构成.png)

响应报文例子：

![响应报文的构成](https://raw.githubusercontent.com/514723273/.md-Pictures/master/响应报文的构成.png)

## 2.3　HTTP 是不保存状态的协议

![HTTP协议自身不具备保存之前发送过的请求或响应的功能](https://raw.githubusercontent.com/514723273/.md-Pictures/master/HTTP协议自身不具备保存之前发送过的请求或响应的功能.png)

## 2.4　请求 URI 定位资源

（粗略）HTTP 协议使用 URI 定位互联网上的资源。正是因为 URI 的特定功能，在互联网上任意位置的资源都能访问到。

## 2.5 告知服务器意图的 HTTP 方法
## 2.6　使用方法下达命令

| 方法 | 说明 | 支持的 HTTP 协议版本 |
| --- | --- | --- |
| GET | 获取资源 | 1.0、1.1 |
| POST | 传输实体主体 | 1.0、1.1 |
| PUT | 传输文件 | 1.0、1.1 |
| HEAD | 获得报文首部 | 1.0、1.1 |
| DELETE | 删除文件 | 1.1 |
| OPTIONS | 追踪路径 | 1.1 |
| TRACE | 追踪路径 | 1.1 |
| CONNECT | 要求用隧道协议连接代理 | 1.1 |
| LINK | 建立和资源之间的联系 | 1.0（1.1 废弃 |
| UNLINE | 断开连接关系 | 1.0（1.1 废弃 |

### 2.6.1 GET 例子

请求：
```
GET /index.html HTTP/1.1
Host: www.hackr.jp
If-Modified-Since: Thu, 12 Jul 2012 07:30:00 GMT
```

响应：
```
仅返回2012年7月12日7点30分以后更新过的index.html页面资源。*如果未有内容更新，则以状态码304 Not Modified作为响应返回*
```

### 2.6.2 POST 例子

请求：
```
POST /submit.cgi HTTP/1.1
Host: www.hackr.jp
Content-Length: 1560（1560字节的数据）
```

响应：
```
返回 submit.cgi 接收数据的处理结果
```

### 2.6.3 PUT 例子

鉴于 HTTP/1.1 的 PUT 方法自身不带验证机制，任何人都可以上传文件 , 存在安全性问题，因此一般的 Web 网站不使用该方法。若配合 Web 应用程序的验证机制，或架构设计采用 REST（REpresentational State Transfer，表征状态转移）标准的同类 Web 网站，就可能会开放使用 PUT 方法。

请求：
```
PUT /example.html HTTP/1.1
Host: www.hackr.jp
Content-Type: text/html
Content-Length: 1560（1560 字节的数据）
```

响应：
```
响应返回状态码 204 No Content（比如 ：该 html 已存在于服务器上）（响应的意思其实是请求执行成功了，但无数据返回）
```

### 2.6.4 HEAD 例子

HEAD 方法和 GET 方法一样，*只是不返回报文主体部分。***用于确认 URI 的有效性及资源更新的日期时间等。**

请求：
```
HEAD /index.html HTTP/1.1
Host: www.hackr.jp
```

响应：
```
返回index.html有关的响应首部
```

### 2.6.5 DELETE 例子

请求：
```
DELETE /example.html HTTP/1.1
Host: www.hackr.jp
```

响应：
```
响应返回状态码 204 No Content（比如 ：该 html 已从该服务器上删除）
```

### 2.6.6 OPTIONS 例子

请求：
```
OPTIONS * HTTP/1.1
Host: www.hackr.jp
```

响应：
```
HTTP/1.1 200 OK
Allow: GET, POST, HEAD, OPTIONS
（返回服务器支持的方法）
```

### 2.6.7 TRACE 例子

*（不怎么会被用到，加上它容易引发 XST（Cross-Site Tracing，跨站追踪）攻击，更用不到了。）*

发送请求时，在 Max-Forwards 首部字段中填入数值，每经过一个服务器端就将该数字减 1，当数值刚好减到 0 时，就停止继续传输，最后接收到请求的服务器端则返回状态码 200 OK 的响应。

客户端通过 TRACE 方法可以查询发送出去的请求是怎样被加工修改 / 篡改的。这是因为，请求想要连接到源目标服务器可能会通过代理中转，TRACE 方法就是用来确认连接过程中发生的一系列操作。

请求：
```
TRACE / HTTP/1.1
Host: hackr.jp
Max-Forwards: 2
```

响应：
```
HTTP/1.1 200 OK
Content-Type: message/http
Content-Length: 1024

TRACE / HTTP/1.1
Host: hackr.jp
Max-Forwards: 2（返回响应包含请求内容）
```

### 2.6.8 CONNECT 例子

CONNECT 方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行 TCP 通信。主要使用 SSL（Secure Sockets Layer，安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加 密后经网络隧道传输。

请求：
```
CONNECT proxy.hackr.jp:8080 HTTP/1.1
Host: proxy.hackr.jp
```

响应：
```
HTTP/1.1 200 OK（之后进入网络隧道）
```

## 2.7 持久连接节省通信量

HTTP 协议的初始版本中，每进行一次 HTTP 通信就要断开一次 TCP 连接。

![每进行一次HTTP通信就要断开一次TCP连接](https://raw.githubusercontent.com/514723273/.md-Pictures/master/每进行一次HTTP通信就要断开一次TCP连接.png)

### 2.7.1　持久连接

持久连接的特点是，只要任意一端没有明确提出断开连接，则保持 TCP 连接状态。（在 HTTP/1.1 中，所有的连接默认都是持久连接）

![持久连接](https://raw.githubusercontent.com/514723273/.md-Pictures/master/持久连接.png)

好处：

持久连接的好处在于减少了 TCP 连接的重复建立和断开所造成的额外开销，减轻了服务器端的负载。另外，减少开销的那部分时间，使 HTTP 请求和响应能够更早地结束，这样 Web 页面的显示速度也就相应提高了。

### 2.7.2 管线化

持久连接使得多数请求以管线化（pipelining）方式发送成为可能。从前发送请求后需等待并收到响应，才能发送下一个请求。管线化技术出现后，不用等待响应亦可直接发送下一个请求。

![管线化](https://raw.githubusercontent.com/514723273/.md-Pictures/master/管线化.png)


### 2.8　使用 Cookie 的状态管理

Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie 的首部字段信息，通知客户端保存 Cookie。当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入 Cookie 值后发送出去。

服务器端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。

没有 Cookie 信息状态下的请求

![没有Cookie信息状态下的请求](https://raw.githubusercontent.com/514723273/.md-Pictures/master/没有Cookie信息状态下的请求.png)

第 2 次以后（存有 Cookie 信息状态）的请求

![第2次以后存有Cookie信息状态的请求](https://raw.githubusercontent.com/514723273/.md-Pictures/master/第2次以后存有Cookie信息状态的请求.png)

# 第 3 章　HTTP 报文内的 HTTP 信息

## 3.1　HTTP 报文

*(用于 HTTP 协议交互的信息被称为 HTTP 报文。请求端（客户端）的 HTTP 报文叫做请求报文，响应端（服务器端）的叫做响应报文。)*

HTTP 报文本身是由多行（用 CR+LF 作**换行**符）数据构成的字符串文本。（空行使用 CR+LF ）

请求报文（上）和响应报文（下）的*结构*

![请求报文和响应报文的结构](https://raw.githubusercontent.com/514723273/.md-Pictures/master/请求报文和响应报文的结构.png)


请求报文（上）和响应报文（下）的*实例*

![请求报文和响应报文的实例](https://raw.githubusercontent.com/514723273/.md-Pictures/master/请求报文和响应报文的实例.png)

{!!!}

报文整体结构
- 报文首部
    - 请求行（方法、URI、HTTP 版本）/状态行（HTTP 版本、状态码）
    - 各种首部字段（通用首部字段、请求首部字段/响应首部字段、实体首部字段）
- 空行
- 报文主体

#### 3.1.1 请求行

包含用于请求的方法，请求 URI 和 HTTP 版本。

#### 3.1.2 状态行

包含表明响应结果的状态码，原因短语和 HTTP 版本。

#### 3.1.3 首部字段

包含表示请求和响应的各种条件和属性的各类首部。

一般有 4 种首部，分别是：通用首部、请求首部、响应首部和实体首部。

#### 3.1.4 其他 

可能包含 HTTP 的 RFC 里未定义的首部（Cookie 等）。

## 3.3 编码提升传输速率

通过在传输时编码，提升传输速率，能有效地处理大量的访问请求。但是，编码的操作需要计算机来完成，因此会消耗更多的 CPU 等资源。

### 3.3.1 报文主体和实体主体的差异

- 报文：是 HTTP 通信中的*基本单位*，由 8 位组字节流（octet sequence，其中 octet 为 8 个比特）组成，通过 HTTP 通信传输。
- 实体：作为请求或响应的有效载荷数据（补充项）被传输，其内容由实体首部和实体主体组成。

通常，报文主体等于实体主体。*只有当传输中进行编码操作时*，实体主体的内容发生变化，才导致它和报文主体产生差异。*（理解术语即可）*

### 3.3.2　压缩传输的内容编码

内容编码指明应用在实体内容上的编码格式，并保持实体信息原样压缩。内容编码后的实体由客户端接收并负责解码。

### 3.3.3 分割发送的分块传输编码

在 HTTP 通信过程中，请求的编码实体资源尚未全部传输完成之前，浏览器无法显示请求页面。在传输大容量数据时，通过把数据分割成多块，能够让浏览器逐步显示页面。

这种把实体主体分块的功能称为分块传输编码（Chunked Transfer Coding）。

## 3.4　发送多种数据的多部分对象集合

HTTP 协议中采纳了多部分对象集合，发送的一份报文主体内可含有多类型实体。通常是在图片或文本文件等上传时使用。

多部分对象集合包含的对象如下：
- multipart/form-data：在 Web *表单*文件上传时使用（请求报文）
    ```
    Content-Type: multipart/form-data; boundary=AaB03x


    --AaB03x


    Content-Disposition: form-data; name=&quot;field1&quot;
    　
    Joe Blow
    --AaB03x


    Content-Disposition: form-data; name=&quot;pics&quot;; filename=&quot;file1.txt&quot;
    Content-Type: text/plain
    　
    ...（file1.txt的数据）...
    --AaB03x--                  // 部分对象集合对应的字符串的最后
    ```
- multipart/byteranges：状态码 206（Partial Content，部分内容）响应报文包含了多个范围的内容时使用。（响应报文）
    ```
    HTTP/1.1 206 Partial Content
    Date: Fri, 13 Jul 2012 02:45:26 GMT
    Last-Modified: Fri, 31 Aug 2007 02:02:20 GMT
    Content-Type: multipart/byteranges; boundary=THIS_STRING_SEPARATES


    --THIS_STRING_SEPARATES


    Content-Type: application/pdf
    Content-Range: bytes 500-999/8000


    ...（范围指定的数据）...
    --THIS_STRING_SEPARATES
    Content-Type: application/pdf
    Content-Range: bytes 7000-7999/8000


    ...（范围指定的数据）...


    --THIS_STRING_SEPARATES--   // 部分对象集合对应的字符串的最后
    ```

    在 HTTP 报文中使用多部分对象集合时，需要在首部字段里加上 Content-type。

    使用 boundary 字符串来划分多部分对象集合指明的各类实体。

    在 boundary 字符串指定的各个实体的起始行之前插入“--”标记（例如：--AaB03x、--THIS_STRING_SEPARATES），

    而在多部分对象集合对应的字符串的最后插入“--”标记（例如：--AaB03x--、--THIS_STRING_SEPARATES--）作为结束。

## 3.5　获取部分内容的范围请求

![范围请求](https://raw.githubusercontent.com/514723273/.md-Pictures/master/范围请求.png)


指定范围发送的请求叫做范围请求（Range Request）。（比如加载一个很大的图片）（还需要一种恢复机制，防止断网）

执行范围请求时，会用到首部字段 Range 来指定资源的 byte 范围。

byte 范围的指定形式如下
- 5001~10000 字节
    ```
    Range: bytes=5001-10000
    ```
- 从 5001 字节之后全部的
    ```
    Range: bytes=5001-
    ```
- 从一开始到 3000 字节和 5000~7000 字节的多重范围
    ```
    Range: bytes=-3000, 5000-7000
    ```
针对范围请求，响应会返回状态码为 206 Partial Content 的响应报文。*(另外，对于多重范围的范围请求，响应会在首部字段 Content-Type 标明 multipart/byteranges 后返回响应报文。)*

如果服务器端无法响应范围请求，则会返回状态码 200 OK 和*完整的实体内容*。

## 3.6　内容协商返回最合适的内容

当浏览器的默认语言为英语或中文，访问相同 URI 的 Web 页面时，则会显示对应的英语版或中文版的 Web 页面。这样的机制称为内容协商（Content Negotiation）。

包含在请求报文中的某些首部字段（如下）就是判断的基准:
- Accept 
- Accept-Charset 
- Accept-Encoding 
- Accept-Language 
- Content-Language

内容协商技术有以下 3 种类型:
- 服务器驱动协商（Server-driven Negotiation） 

    由服务器端进行内容协商。以请求的首部字段为参考，在服务器端自动处理。但对用户来说，以浏览器发送的信息作为判定的依据，并不一定能筛选出最优内容。
- 客户端驱动协商（Agent-driven Negotiation） 

    由客户端进行内容协商的方式。用户从浏览器显示的可选项列表中手动选择。还可以利用 JavaScript 脚本在 Web 页面上自动进行上述选择。比如按 OS 的类型或浏览器类型，自行切换成 PC 版页面或手机版页面。
- 透明协商（Transparent Negotiation） 

    是服务器驱动和客户端驱动的**结合体**，是由服务器端和客户端各自进行内容协商的一种方法。

# 第 4 章　返回结果的 HTTP 状态码

## 4.1　状态码告知从服务器端返回的请求结果

| 状态码 |类别 | 原因短语 |
| --- | --- | --- |
|1XX|Informational（信息性状态码）|接收的请求正在处理|
|2XX|Success（成功状态码）|请求正常处理完毕|
|3XX|Redirection（重定向状态码）|需要进行附加操作以完成请求|
|4XX|Client Error（客户端错误状态码）|服务器无法处理请求|
|5XX|Server Error（服务器错误状态码）|服务器处理请求出错|

数量达 60 余种，别看种类繁多，实际上经常使用的大概只有 14 种。接下来，我们就介绍一下这些具有代表性的 14 个状态码。

## 4.2　2XX 成功

- 200 OK 表示从客户端发来的请求在服务器端被正常处理了。
- 204 No Content 该状态码代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外，也不允许返回任何实体的主体。
- 206 Partial Content 该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的 GET 请求。响应报文中包含由 Content-Range 指定范围的实体内容。

## 4.3 3XX 重定向

- 301 Moved Permanently 永久性重定向。该状态码表示请求的资源已被分配了新的 URI，以后应使用资源现在所指的 URI。
- 302 Found 临时性重定向。该状态码表示请求的资源已被分配了新的 URI，希望用户（本次）能使用新的 URI 访问。
- 303 See Other 该状态码表示由于请求对应的资源存在着另一个 URI，应使用 **GET 方法** 定向获取请求的资源。
- 304 Not Modified 该状态码表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但未满足条件的情况。304 状态码返回时，不包含任何响应的主体部分。*（附带条件的请求是指采用 GET 方法的请求报文中包含 If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since 中任一首部。）*
- 307 Temporary Redirect 临时重定向。该状态码与 302 Found 有着相同的含义。307 会遵照浏览器标准，不会从 POST 变成 GET。

当 301、302、（前两个禁止，但实际还是使用）303 响应状态码返回时，几乎所有的浏览器都会把 POST 改成 GET，并删除请求报文内的主体，之后请求会自动再次发送。

## 4.4 4XX 客户端错误

- 400 Bad Request 该状态码表示请求报文中存在语法错误。
- 401 Unauthorized 该状态码表示发送的请求需要有通过 HTTP 认证（BASIC 认证、DIGEST 认证）的认证信息。
- 403 Forbidden 该状态码表明对请求资源的访问被服务器拒绝了。
- 404 Not Found 该状态码表明服务器上无法找到请求的资源。

## 4.5　5XX 服务器错误

- 500 Internal Server Error 该状态码表明服务器端在执行请求时发生了错误。
- 503 Service Unavailable 该状态码表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。

返回含有 401 的响应必须包含一个适用于被请求资源的 WWW-Authenticate 首部用以质询（challenge）用户信息。当浏览器初次接收到 401 响应，会弹出认证用的对话窗口。

状态码和状况的不一致：不少返回的状态码响应都是错误的，但是用户可能察觉不到这点。比如 Web 应用程序内部发生错误，状态码依然返回 200 OK，这种情况也经常遇到。

# 第 5 章　与 HTTP 协作的 Web 服务器

## 5.1　用单台虚拟主机实现多个域名

HTTP/1.1 规范允许一台 HTTP 服务器搭建多个 Web 站点。

即使物理层面只有一台服务器，但只要使用虚拟主机的功能，则可以假想已具有多台服务器。

在互联网上，域名通过 DNS 服务映射到 IP 地址（域名解析）之后访问目标网站。可见，当请求发送到服务器时，已经是以 IP 地址形式访问了，会出现如图情况：

![虚拟主机不同域名相同IP](https://raw.githubusercontent.com/514723273/.md-Pictures/master/虚拟主机不同域名相同IP.png)

所以在相同的 IP 地址下，由于虚拟主机可以寄存多个不同主机名和域名的 Web 网站，因此在发送 HTTP 请求时，必须在 Host 首部内完整指定主机名或域名的 URI。

## 5.2　通信数据转发程序 ：代理、网关、隧道

HTTP 通信时，除客户端和服务器以外，还有一些用于通信数据转发的**应用程序**，例如代理、网关和隧道。它们可以配合服务器工作。

- 代理：是一种有转发功能的应用程序，它扮演了位于服务器和客户端“中间人”的角色，接收由客户端发送的请求并转发给服务器，同时也接收服务器返回的响应并转发给客户端。
- 网关：是转发其他服务器通信数据的服务器，接收从客户端发送来的请求时，它就像自己拥有资源的源服务器一样对请求进行处理。有时客户端可能都不会察觉，自己的通信目标是一个网关。
- 隧道：是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序。

### 5.2.1　代理

代理服务器的基本行为就是接收客户端发送的请求后转发给其他服务器。代理不改变请求 URI，会直接发送给前方持有资源的目标服务器。

![代理服务器](https://raw.githubusercontent.com/514723273/.md-Pictures/master/代理服务器.png)

每次通过代理服务器转发请求或响应时，会追加写入 Via 首部信息

使用代理服务器的*理由*有：利用缓存技术减少网络带宽的流量，组织内部针对特定网站的访问控制，以获取访问日志为主要目的，等等。

代理有多种使用方法，按两种基准分类。一种是是否使用缓存，另一种是是否会修改报文。
- 缓存代理 代理转发响应时，缓存代理（Caching Proxy）会预先将资源的副本（缓存）保存在代理服务器上。当代理再次接收到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回。
- 透明代理 转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理（Transparent Proxy）。反之，对报文内容进行加工的代理被称为非透明代理。

### 5.2.2　网关

![网关](https://raw.githubusercontent.com/514723273/.md-Pictures/master/网关.png)

网关的工作机制和代理十分相似。而网关能使通信线路上的服务器提供**非 HTTP** 协议服务。

利用网关能提高通信的安全性，因为可以在客户端与网关之间的通信线路上加密以确保连接的安全。

### 5.2.3　隧道

![隧道](https://raw.githubusercontent.com/514723273/.md-Pictures/master/隧道.png)

隧道可按要求建立起一条与其他服务器的通信线路，届时使用 SSL 等加密手段进行通信。隧道的目的是确保客户端能与服务器进行安全的通信。

隧道本身不会去解析 HTTP 请求。

## 5.3　保存资源的缓存

缓存是指代理服务器或客户端本地磁盘内保存的资源副本。利用缓存可减少对源服务器的访问，因此也就节省了通信流量和通信时间。

![缓存服务器](https://raw.githubusercontent.com/514723273/.md-Pictures/master/缓存服务器.png)

### 5.3.1　缓存的有效期限

![缓存的有效期限](https://raw.githubusercontent.com/514723273/.md-Pictures/master/缓存的有效期限.png)

即使存在缓存，也会因为客户端的要求、缓存的有效期等因素，向源服务器确认资源的有效性。若判断缓存失效，缓存服务器将会再次从源服务器上获取“新”资源。

### 5.3.2　客户端的缓存

浏览器缓存如果有效，就不必再向服务器请求相同的资源了，可以直接从本地磁盘内读取。

另外，和缓存服务器相同的一点是，当判定缓存过期后，会向源服务器确认资源的有效性。若判断浏览器缓存失效，浏览器会再次请求新资源。

![客户端的缓存](https://raw.githubusercontent.com/514723273/.md-Pictures/master/客户端的缓存.png)

# 第 6 章　HTTP 首部

## 6.2　HTTP 首部字段

### 6.2.1　HTTP 首部字段传递重要信息

使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容。

### 6.2.2　HTTP 首部字段结构

HTTP 首部字段是由首部字段名和字段值构成的，中间用冒号“:” 分隔。（首部字段名: 字段值）

（若 HTTP 首部字段重复了会如何？内尚未明确。有些浏览器会优先处理第一次出现的首部字段，而有些则会优先处理最后出现的首部字段。）
```
// 也可以多值
Keep-Alive: timeout=15, max=100
```

### 6.2.3　4 种 HTTP 首部字段类型

HTTP 首部字段根据实际用途被分为以下 4 种类型：
- 通用首部字段：请求报文和响应报文两方都会使用的首部。
- 请求首部字段：从*客户端*向服务器端发送请求报文时使用的首部。补充了请求的附加内容、客户端信息、响应内容相关优先级等信息。
- 响应首部字段：从*服务器端*向客户端返回响应报文时使用的首部。补充了响应的附加内容，也会要求客户端附加额外的内容信息。
- 实体首部字段：针对请求报文和响应报文的*实体部分*使用的首部。补充了资源内容更新时间等与实体有关的信息。

### 6.2.4　HTTP/1.1 首部字段一览

HTTP/1.1 规范定义了如下 **47 种**首部字段。

**通用首部字段**

| 首部字段名 | 说明 |
| --- | --- |
|Cache-Control|控制缓存的行为|
|Connection|逐跳首部、连接的管理|
|Date|创建报文的日期时间|
|Pragma|报文指令|
|Trailer|报文末端的首部一览|
|Transfer-Encoding|指定报文主体的传输编码方式|
|Upgrade|升级为其他协议|
|Via|代理服务器的相关信息|
|Warning|错误通知|

**请求首部字段**

|首部字段名|说明|
|Accept|用户代理可处理的媒体类型|
|Accept-Charset|优先的字符集|
|Accept-Encoding|优先的内容编码|
|Accept-Language|优先的语言（自然语言）|
|Authorization|Web认证信息|
|Expect|期待服务器的特定行为|
|From|用户的电子邮箱地址|
|Host|请求资源所在服务器|
|If-Match|比较实体标记（ETag）|
|If-Modified-Since|比较资源的更新时间|
|If-None-Match|比较实体标记（与 If-Match 相反）|
|If-Range|资源未更新时发送实体 Byte 的范围请求|
|If-Unmodified-Since|比较资源的更新时间（与If-Modified-Since相反）|
|Max-Forwards|最大传输逐跳数|
|Proxy-Authorization|代理服务器要求客户端的认证信息|
|Range|实体的字节范围请求|
|Referer|对请求中 URI 的原始获取方|
|TE|传输编码的优先级|
|User-Agent|HTTP 客户端程序的信息|

**响应首部字段**

|首部字段名|说明|
|Accept-Ranges|是否接受字节范围请求|
|Age|推算资源创建经过时间|
|ETag|资源的匹配信息|
|Location|令客户端重定向至指定URI|
|Proxy-Authenticate|代理服务器对客户端的认证信息|
|Retry-After|对再次发起请求的时机要求|
|Server|HTTP服务器的安装信息|
|Vary|代理服务器缓存的管理信息|
|WWW-Authenticate|服务器对客户端的认证信息|

**实体首部字段**

|首部字段名|说明 |
|Allow|资源可支持的HTTP方法|
|Content-Encoding|实体主体适用的编码方式|
|Content-Language|实体主体的自然语言|
|Content-Length|实体主体的大小（单位：字节）|
|Content-Location|替代对应资源的URI|
|Content-MD5|实体主体的报文摘要|
|Content-Range|实体主体的位置范围|
|Content-Type|实体主体的媒体类型|
|Expires|实体主体过期的日期时间|
|Last-Modified|资源的最后修改日期时间|

### 6.2.5　非 HTTP/1.1 首部字段

在 HTTP 协议通信交互中使用到的首部字段，不限于 RFC2616 中定义的 47 种首部字段。

还有 Cookie、Set-Cookie 和 Content-Disposition 等在其他 RFC 中定义的首部字段，它们的使用频率也很高。

### 6.2.6　End-to-end 首部和 Hop-by-hop 首部

（没懂）

HTTP 首部字段将定义成**缓存代理**和**非缓存代理**的行为，分成 2 种类型。

**端到端首部（End-to-end Header）**

分在此类别中的首部会转发给请求 / 响应对应的最终接收目标，且必须保存在由缓存生成的响应中，另外规定它必须被转发。

**逐跳首部（Hop-by-hop Header）** 

分在此类别中的首部只对单次转发有效，会因通过缓存或代理而不再转发。HTTP/1.1 和之后版本中，如果要使用 hop-by-hop 首部，需提供 Connection 首部字段。
下面列举了 HTTP/1.1 中的逐跳首部字段。除这 8 个首部字段之外，其他所有字段都属于端到端首部。
- Connection 
- Keep-Alive 
- Proxy-Authenticate 
- Proxy-Authorization 
- Trailer 
- TE 
- Transfer-Encoding 
- Upgrade

## 6.3　HTTP/1.1 通用首部字段

### 6.3.1　Cache-Control

### 6.3.2　Connection

### 6.3.3　Date

### 6.3.4　Pragma

### 6.3.5　Trailer

### 6.3.6　Transfer-Encoding

### 6.3.7　Upgrade

### 6.3.8　Via

### 6.3.9　Warning

## 6.4　请求首部字段

### 6.4.1　Accept

### 6.4.2　Accept-Charset

### 6.4.3　Accept-Encoding

### 6.4.5　Authorization

### 6.4.6　Expect

### 6.4.7　From

### 6.4.8　Host

### 6.4.9　If-Match

只有当 If-Match 的字段值跟 ETag 值匹配一致时，服务器才会接受请求。

![If-Match](https://raw.githubusercontent.com/514723273/.md-Pictures/master/If-Match.png)

### 6.4.10　If-Modified-Since

![If-Modified-Since](https://raw.githubusercontent.com/514723273/.md-Pictures/master/If-Modified-Since.png)

### 6.4.11　If-None-Match

### 6.4.12　If-Range

![If-Range](https://raw.githubusercontent.com/514723273/.md-Pictures/master/If-Range.png)

### 6.4.13　If-Unmodified-Since

### 6.4.14　Max-Forwards

### 6.4.15　Proxy-Authorization

### 6.4.16　Range

### 6.4.17　Referer

### 6.4.18　TE

首部字段 TE 会告知服务器客户端能够处理响应的传输编码方式及相对优先级。它和首部字段 Accept-Encoding 的功能很相像，但是用于传输编码。
```
TE: gzip, deflate;q=0.5
```

### 6.4.19　User-Agent

首部字段 User-Agent 会将创建请求的浏览器和用户代理名称等信息传达给服务器。

```
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:13.0) Gecko/20100101 Firefox/13.0.1
```

## 6.5　响应首部字段

### 6.5.1　Accept-Ranges

首部字段 Accept-Ranges 是用来告知客户端服务器是否能处理范围请求，以指定获取服务器端某个部分的资源。

可指定的字段值有两种，可处理范围请求时指定其为 bytes，反之则指定其为 none。
```
Accept-Ranges: bytes
```

