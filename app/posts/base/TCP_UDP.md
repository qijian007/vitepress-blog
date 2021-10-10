--- 
title: TCP和UDP
date: 2020-02-29
tags:  
- 网络
categories:
- 计算机基础
---
## TCP
传输控制协议（`TCP`）是一种面向连接的、可靠的、基于字节流的传输层通信协议，由`IETF`的`RFC 793`定义。在简化的计算机网络`OSI`模型中的传输层。

### TCP三次握手过程
![](https://raw.githubusercontent.com/Moking1997/NotePhoto/master/20200229225736.png)
1. 客户端通过向服务器端发送一个`SYN`来创建一个主动打开，作为三次握手的一部分。客户端把这段连接的序号设定为随机数X。
2. 服务器端应当为一个合法的`SYN`回送一个`SYN/ACK`。`ACK`的确认码应为X+1，`SYN/ACK`包本身又有一个随机产生的序号Y。
3. 最后，客户端再发送一个`ACK`。此时包的序号被设定为X+1，而`ACK`的确认码则为Y+1。当服务端收到这个`ACK`的时候，就完成了三次握手，并进入了连接创建状态。

3次握手的特点

没有应用层的数据 ,SYN这个标志位只有在TCP建立连接时才会被置1 ,握手完成后SYN标志位被置0。

### TCP的四次握手
![](https://raw.githubusercontent.com/Moking1997/NotePhoto/master/20200229230940.png)
1. 当 客户端完成数据传输后,将控制位FIN置1，提出停止TCP连接的请求 

2. 服务器收到FIN后对其作出响应，确认这一方向上的TCP连接将关闭,将ACK置1

3. 由服务器再提出反方向的关闭请求,将FIN置1 

4.  客户端对服务器的请求进行确认，将ACK置1，双方向的关闭结束

### TCP的表头
![](https://raw.githubusercontent.com/Moking1997/NotePhoto/master/20200229221716.png)
- 序列号（seq，32位长）
    - 如果含有同步化旗标（SYN），则此为最初的序列号,第一个数据比特的序列码为本序列号加一。
    - 如果没有同步化旗标（SYN），则此为第一个数据比特的序列码。
- 窗口（WIN，16位长）
    - 表示从确认号开始，本报文的发送方可以接收的字节数，即接收窗口大小。用于流量控制
- 标识符
    - URG=1：该字段为一表示本数据报的数据部分包含紧急信息，是一个高优先级数据报文，此时紧急指针有效。紧急数据一定位于当前数据包数据部分的最前面，紧急指针标明了紧急数据的尾部。
    - ACK=1：该字段为一表示确认号字段有效。此外，TCP 还规定在连接建立后传送的所有报文段都必须把 ACK 置为一。
    - PSH=1：该字段为一表示接收端应该立即将数据 push 给应用层，而不是等到缓冲区满后再提交。
    - RST=1：该字段为一表示当前 TCP 连接出现严重问题，可能需要重新建立 TCP 连接，也可以用于拒绝非法的报文段和拒绝连接请求。
    - SYN=1：当SYN=1，ACK=0时，表示当前报文段是一个连接请求报文。当SYN=1，ACK=1时，表示当前报文段是一个同意建立连接的应答报文。
    - FIN=1：该字段为一表示此报文段是一个释放连接的请求报文。
## UDP
用户数据报协议（`UDP`）是同一层内另一个重要的传输协议,是一个简单的面向数据报的通信协议,是一种无连接的协议。

**`UDP`有不提供数据包分组、组装和不能对数据包进行排序的缺点**,也就是说，当报文发送之后，是无法得知其是否安全完整到达的。

### 特点
1. **`UDP`是无连接的**，即发送数据之前不需要建立连接，因此减少了开销和发送数据之前的时延。

2. **`UDP`是不可靠的**,因为`UDP`使用尽最大努力交付，即不保证可靠交付，因此主机不需要维持复杂的连接状态表。

3. **`UDP`是面向报文的**。发送方的`UDP`对应用程序交下来的报文，在添加首部后就向下交付IP层。`UDP`对应用层交下来的报文，既不合并，也不拆分，而是保留这些报文的边界。因此，应用程序必须选择合适大小的报文。
4. **`UDP`没有拥塞控制**,因此网络出现的拥塞不会使源主机的发送速率降低。很多的实时应用（如IP电话、实时视频会议等）要去源主机以恒定的速率发送数据，并且允许在网络发生拥塞时丢失一些数据，但却不允许数据有太多的时延。`UDP`正好符合这种要求。

5. `UDP`支持一对一、一对多、多对一和多对多的交互通信。

6. `UDP`的首部开销小，只有8个字节，比TCP的20个字节的首部要短。
![](https://raw.githubusercontent.com/Moking1997/NotePhoto/master/20200229221347.png)
- 源端口:在需要对方回信时选用。不需要时可用全0。

- 目的端口:这在终点交付报文时必须要使用到。

- 长度:UDP用户数据报的长度，其最小值是8（仅有首部）

- 检验和:检测UDP用户数据报在传输中是否有错。有错就丢弃。


### `TCP` 与 `UDP` 的区别
1. `TCP`基于连接,`UDP`是无连接

2. 对系统资源的要求（`TCP`较多，`UDP`少）

3. `UDP`程序结构较简单,速度快

4. 流模式与数据报模式 

5. `TCP`保证数据正确性，`UDP`可能丢包

6. `TCP`保证数据顺序，`UDP`不保证。