---
layout: post
title: "需要知道的http知识"
description: 工作中http真的是太常见，需要对此有一个充分的学习！
date:   2017-09-12 10:50:00 +0800
categories: http
author: bwzou
---
随着Web应用程序功能越来越丰富，开发者对web的要求开始变得多了起来。这就需要深入了解支撑Web基础的HTTP协议。对HTTP协议有了更深入的理解后，对开发也会有一些启发。

## 基本概念和相关协议
#### 与 HTTP 关系密切的协议 : IP、TCP 和 DNS
1、**负责传输的 IP 协议**

按层次分，IP（Internet Protocol）网际协议位于网络层。Internet Protocol 这个名称可能听起来有点夸张，但事实正是如此，因为几乎所有使用网络的系统都会用到 IP 协议。TCP/IP 协议族中的 IP 指的就是网际协议，协议名称中占据了一半位置，其重要性可见一斑。可能有人会把“IP”和“IP 地址”搞混，“IP”其实是一种协议的名称。

IP 协议的作用是把各种数据包传送给对方。而要保证确实传送到对方那里，则需要满足各类条件。其中两个重要的条件是 IP 地址和 MAC 地址（Media Access Control Address）。

IP 地址指明了节点被分配到的地址，MAC 地址是指网卡所属的固定地址。IP 地址可以和 MAC 地址进行配对。IP 地址可变换，但 MAC 地址基本上不会更改。

**使用 ARP 协议凭借 MAC 地址进行通信**

IP 间的通信依赖 MAC 地址。在网络上，通信的双方在同一局域网（LAN）内的情况是很少的，通常是经过多台计算机和网络设备中转才能连接到对方。而在进行中转时，会利用下一站中转设备的 MAC 地址来搜索下一个中转目标。这时，会采用 ARP 协议（Address Resolution Protocol）。ARP 是一种用以解析地址的协议，根据通信方的 IP 地址就可以反查出对应的 MAC 地址。

2、**确保可靠性的 TCP 协议**

按层次分，TCP 位于传输层，提供可靠的字节流服务。
所谓的字节流服务（Byte Stream Service）是指，为了方便传输，将大块数据分割成以报文段（segment）为单位的数据包进行管理。而可靠的传输服务是指，能够把数据准确可靠地传给对方。一言以蔽之，TCP 协议为了更容易传送大数据才把数据分割，而且 TCP 协议能够确认数据最终是否送达到对方。

**确保数据能到达目标**

为了准确无误地将数据送达目标处，TCP 协议采用了三次握手（three-way handshaking）策略。用 TCP 协议把数据包送出去后，TCP 不会对传送后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了 TCP 的标志（flag） —— SYN（synchronize） 和 ACK（acknowledgement）。

发送端首先发送一个带 SYN 标志的数据包给对方。接收端收到后，回传一个带有 SYN/ACK 标志的数据包以示传达确认信息。最后，发送端再回传一个带 ACK 标志的数据包，代表“握手”结束

3、**负责域名解析的 DNS 服务**

DNS（Domain Name System）服务是和 HTTP 协议一样位于应用层的协议。它提供域名到 IP 地址之间的解析服务。

计算机既可以被赋予 IP 地址，也可以被赋予主机名和域名。比如 google.com。

用户通常使用主机名或域名来访问对方的计算机，而不是直接通过 IP 地址访问。因为与 IP 地址的一组纯数字相比，用字母配合数字的表示形式来指定计算机名更符合人类的记忆习惯。

但要让计算机去理解名称，相对而言就变得困难了。因为计算机更擅长处理一长串数字。

为了解决上述的问题，DNS 服务应运而生。DNS 协议提供通过域名查找 IP 地址，或逆向从 IP 地址反查域名的服务。

#### URI 和 URL
URL（Uniform Resource Locator，统一资源定位符） 正是使用 Web 浏览器等访问 Web 页面时需要输入的网页地址。比如 http://google.com/ 就是 URL。

URI 是 Uniform Resource Identifier 的缩写。RFC2396 分别对这 3 个单词进行了如下定义。
1、Uniform

规定统一的格式可方便处理多种不同类型的资源，而不用根据上下文环境来识别资源指定的访问方式。另外，加入新增的协议方案（如 http: 或 ftp:）也更容易。

2、Resource

资源的定义是“可标识的任何东西”。除了文档文件、图像或服务（例如当天的天气预报）等能够区别于其他类型的，全都可作为资源。另外，资源不仅可以是单一的，也可以是多数的集合体。

3、Identifier

表示可标识的对象。也称为标识符。
综上所述，URI 就是由某个协议方案表示的资源的定位标识符。协议方案是指访问资源所使用的协议类型名称。


## 简单HTTP协议
HTTP 是一种不保存状态，即无状态（stateless）协议。HTTP 协议自身不对请求和响应之间的通信状态进行保存。也就是说在 HTTP 这个级别，协议对于发送过的请求或响应都不做持久化处理。

HTTP 协议使用 URI 定位互联网上的资源。正是因为 URI 的特定功能，在互联网上任意位置的资源都能访问到。
#### 请求方法
1、 **GET** 

GET 是最常用的方法。通常用于请求服务器发送某个资源。HTTP/1.1 要求服务器实现此方法。

2、 **PUT** 
 
与 GET 从服务器读取文档相反，PUT 方法会向服务器写入文档。PUT 方法的语义就是让服务器用请求的主体部分来创建一个由所请求的 URL 命名的新文档，或者，如果那个 URL 已经存在的话，就用这个主体来替代它。

3、 **POST**

POST 方法起初是用来向服务器输入数据的。实际上，通常会用它来支持 HTML 的表单。表单中填好的数据通常会被送给服务器，然后由服务器将其发送到它要去的地方。

4、 **TRACE** 

客户端发起一个请求时，这个请求可能要穿过防火墙、代理、网关或其他一些应用程序。每个中间节点都可能会修改原始的 HTTP 请求。

TRACE 请求会在目的服务器端发起一个“环回”诊断。行程最后一站的服务器会弹回一条 TRACE 响应，并在响应主体中携带它收到的原始请求报文。这样客户端就可以查看在所有中间 HTTP 应用程序组成的请求 / 响应链上，原始报文是否，以及如何被毁坏或修改过

5、 **OPTIONS**

OPTIONS 方法请求 Web 服务器告知其支持的各种功能。可以询问服务器通常支持哪些方法，或者对某些特殊资源支持哪些方法。（有些服务器可能只支持对一些特殊类型的对象使用特定的操作）。

6、 **DELETE**

顾名思义，DELETE 方法所做的事情就是请服务器删除请求 URL 所指定的资源。但是，客户端应用程序无法保证删除操作一定会被执行。因为 HTTP 规范允许服务器在不通知客户端的情况下撤销请求。

## HTTP报文
#### HTTP首部字段
HTTP 首部字段是由首部字段名和字段值构成的，中间用冒号“:” 分隔。

	首部字段名: 字段值

HTTP 首部字段根据实际用途被分为以下 4 种类型。

1、通用首部字段

首部字段名   		| 说明
----------------|-------
Cache-Control   |控制缓存的行为 
 Connection 	    |逐跳首部、连接的管理 
 Date 	 		|创建报文的日期时间 
 Pragma 	 		|报文指令 
 Trailer 	 	|报文末端的首部一览 
 Transfer-Encoding 	 |指定报文主体的传输编码方式 
 Upgrade 	 	|升级为其他协议 
 Via 	 		|代理服务器的相关信息 
 Warning 	 	|错误通知

2、请求首部字段

首部字段名   		| 说明
----------------|-------
Accept 	 		|用户代理可处理的媒体类型 
 Accept-Charset 	|优先的字符集 
 Accept-Encoding|优先的内容编码 
 Accept-Language|优先的语言（自然语言） 
 Authorization Web	| 	认证信息 
 Expect 	 		|期待服务器的特定行为 
 From 	 		|用户的电子邮箱地址 
 Host 	 		|请求资源所在服务器 
 If-Match 	 	|比较实体标记（ETag）
If-Modified-Since 	|	比较资源的更新时间 
 If-None-Match 	|比较实体标记（与 If-Match 相反） 
 If-Range 	 	|资源未更新时发送实体 Byte 的范围请求 
 If-Unmodified-Since|	比较资源的更新时间（与If-Modified-Since相反） 
 Max-Forwards 	|最大传输逐跳数 
 Proxy-Authorization|	代理服务器要求客户端的认证信息 
 Range 	 		|实体的字节范围请求 
 Referer 	 	|对请求中 URI 的原始获取方 
 TE 	 			|传输编码的优先级 
 User-Agent 	 HTTP 	|	客户端程序的信息

3、响应首部字段

首部字段名   		| 说明
----------------|------------------
Accept-Ranges 	|是否接受字节范围请求 
 Age 	 		|推算资源创建经过时间 
 ETag 	 		|资源的匹配信息 
 Location 	 	|令客户端重定向至指定URI 
 Proxy-Authenticate 	 |	代理服务器对客户端的认证信息 
 Retry-After 	|对再次发起请求的时机要求 
 Server 	 		|HTTP服务器的安装信息 
 Vary 	 		|代理服务器缓存的管理信息 
 WWW-Authenticate 	 |	服务器对客户端的认证信息

4、实体首部字段

首部字段名		| 说明
----------------|-------
Allow 	 		|资源可支持的HTTP方法 
 Content-Encoding	| 实体主体适用的编码方式 
 Content-Language 	| 实体主体的自然语言 
 Content-Length 	|实体主体的大小（单位：字节） 
 Content-Location 	| 替代对应资源的URI 
 Content-MD5 	|实体主体的报文摘要 
 Content-Range 	|实体主体的位置范围 
 Content-Type 	|实体主体的媒体类型 
 Expires 	 	|实体主体过期的日期时间
Last-Modified 	|资源的最后修改日期时间

在 HTTP 协议通信交互中使用到的首部字段，不限于 RFC2616 中定义的 47 种首部字段。还有Cookie、Set-Cookie 和 Content-Disposition 等在其他 RFC 中定义的首部字段，它们的使用频率也很高。这些非正式的首部字段统一归纳在 RFC4229 HTTP Header Field Registrations 中。

#### 为 Cookie 服务的首部字段
管理服务器与客户端之间状态的 Cookie，虽然没有被编入标准化 HTTP/1.1 的 RFC2616 中，但在 Web 网站方面得到了广泛的应用。

Cookie 的工作机制是用户识别及状态管理。Web 网站为了管理用户的状态会通过 Web 浏览器，把一些数据临时写入用户的计算机内。接着当用户访问该Web网站时，可通过通信方式取回之前发放的 Cookie。

调用 Cookie 时，由于可校验 Cookie 的有效期，以及发送方的域、路径、协议等信息，所以正规发布的 Cookie 内的数据不会因来自其他 Web 站点和攻击者的攻击而泄露。

首部字段名	| 	 说明 						| 首部类型 
------------|-------------------------------|---------------
 Set-Cookie 	| 开始状态管理所使用的Cookie信息 	| 响应首部字段 
 Cookie 	 	| 服务器接收到的Cookie信息 	 	| 请求首部字段

**Set-Cookie** 

	SetCookie: status=enable; expires=Tue, 05 Jul 2011 07:26:31 GMT; path=/; domain=Google.com

当服务器准备开始管理客户端的状态时，会事先告知各种信息。下面的表格列举了 Set-Cookie 的字段值。

属性			| 	 说明 
------------|------------------------------------------------
 NAME=VALUE 	|	赋予 Cookie 的名称和其值（必需项） 
expires=DATE| 	 Cookie 的有效期（若不明确指定则默认为浏览器关闭前为止） 
 path=PATH 	|	将服务器上的文件目录作为Cookie的适用对象（若不指定则默认为文档所在的文件目录） 
 domain=域名	|	作为 Cookie 适用对象的域名 （若不指定则默认为创建 Cookie 的服务器的域名） 
 Secure 	 	|	仅在 HTTPS 安全通信时才会发送 Cookie 
 HttpOnly 	|	加以限制，使 Cookie 不能被 JavaScript 脚本访问

1、expires 属性

Cookie 的 expires 属性指定浏览器可发送 Cookie 的有效期。

当省略 expires 属性时，其有效期仅限于维持浏览器会话（Session）时间段内。这通常限于浏览器应用程序被关闭之前。

另外，一旦 Cookie 从服务器端发送至客户端，服务器端就不存在可以显式删除 Cookie 的方法。但可通过覆盖已过期的 Cookie，实现对客户端 Cookie 的实质性删除操作。

2、path 属性

Cookie 的 path 属性可用于限制指定 Cookie 的发送范围的文件目录。不过另有办法可避开这项限制，看来对其作为安全机制的效果不能抱有期待。

3、domain 属性

通过 Cookie 的 domain 属性指定的域名可做到与结尾匹配一致。比如，当指定 example.com 后，除 example.com 以外，www.example.com 或 www2.example.com 等都可以发送 Cookie。

因此，除了针对具体指定的多个域名发送 Cookie 之 外，不指定 domain 属性显得更安全。

4、secure 属性

Cookie 的 secure 属性用于限制 Web 页面仅在 HTTPS 安全连接时，才可以发送 Cookie。
发送 Cookie 时，指定 secure 属性的方法如下所示。

	Set-Cookie: name=value; secure

以上例子仅当在 https://www.example.com/（HTTPS）安全连接的情况下才会进行 Cookie 的回收。也就是说，即使域名相同，http://www.example.com/（HTTP）也不会发生 Cookie 回收行为。

当省略 secure 属性时，不论 HTTP 还是 HTTPS，都会对 Cookie 进行回收。

5、HttpOnly 属性

Cookie 的 HttpOnly 属性是 Cookie 的扩展功能，它使 JavaScript 脚本无法获得 Cookie。其主要目的为防止跨站脚本攻击（Cross-site scripting，XSS）对 Cookie 的信息窃取。

发送指定 HttpOnly 属性的 Cookie 的方法如下所示。

	Set-Cookie: name=value; HttpOnly

通过上述设置，通常从 Web 页面内还可以对 Cookie 进行读取操作。但使用 JavaScript 的 document.cookie 就无法读取附加 HttpOnly 属性后的 Cookie 的内容了。因此，也就无法在 XSS 中利用 JavaScript 劫持 Cookie 了。

虽然是独立的扩展功能，但 Internet Explorer 6 SP1 以上版本等当下的主流浏览器都已经支持该扩展了。另外顺带一提，该扩展并非是为了防止 XSS 而开发的。

**Cookie**

	Cookie: status=enable

首部字段 Cookie 会告知服务器，当客户端想获得 HTTP 状态管理支持时，就会在请求中包含从服务器接收到的 Cookie。接收到多个 Cookie 时，同样可以以多个 Cookie 形式发送。

#### 返回状态码
HTTP 状态码负责表示客户端 HTTP 请求的返回结果、标记服务器端的处理是否正常、通知出现的错误等工作。

状态码	 |             类别             	|       短语原因           
---------|------------------------------|--------------------
 1xx     |Informational(信息性状态码)    | 接收的请求正在处理         
 2xx     |Success（成功状态码)           | 请求正常处理完毕           
 3XX     |Redirection(重定向状态码)      | 需要进行附加操作以完成请求  
 4XX     |Client Error（客户端错误状态码）| 服务器无法处理请求          
 5XX     |Server Error（服务器错误状态码）| 服务器处理请求出错    

200 OK

204 No Content

206 Partial Content

301 Moved Permanently

302 Found

303 See Other

304 Not Modified

307 Temporary Redirect

400 Bad Request

401 Unauthorized

403 Forbidden

404 Not Found

500 Internal Server Error

503 Service Unavailable


参考书籍：《图解HTTP》、《HTTP权威指南》(图灵程序设计丛书)