- 保证送达
- 双工

## TCP 过程

1. 建立连接
   > 服务器先行，先进入listen状态
   >
   > 
   - 三次握手
     - syn，seqnum
       - 服务端接收后会在队列中添加一个节点
         - 一共两个队列
           - 半连接队列（SYN队列）
           - 全连接队列（Accept队列）
     - ack，acknum，seqnum
       - acknum基于对方发过来的，表示接收到
       - 自己的seqnum单列
       - 各论各的
     - ack，acknum
       - 可以带数据
   - connect函数调用
     - 无数据
   - send函数调用
     - 有数据
     - 可以与第三次握手一起发送



1. 数据传输
2. 断开连接

- seqnum
  - 本端发送的值
- acknum
  - 本端接收的值
- 

## client与server行为

### server

- socket
- bind()ip:端口 - optional
  - 端口默认80
  - 不绑定会随机分配端口
  - 最好要绑定端口
  - ip绑定0.0.0.0，可以监听所有网卡网络 - 可以使外网机器连接之
    - 但绑定127.0.0.1只可以监听本机网络，则外网机器无法连接之
- listen(fd, backlog);

  - backlog的含义按照版本不同，可能有：

    - 限制全连接的节点数

      - 仅Accept队列

    - 限制半连接加全连接的节点数

      - SYN队列+Accept队列

    - > 缓冲区中排队等待的未处理连接的最大数量

- accept
- recv
- send
- close

### client 

- socket
- bind(); - optional
- connect();
- send
- recv
- close

## 自定义协议

- 二进制协议

  - 定长
    - TCP/UDP可用，因为可直接按长度分包，无需保证顺序

- 字符

  - 不定长

    - 分包

      - 加“长度”
      - 或结尾标识

      > 基于TCP的顺序特性

    - 仅TCP可用，因为需保持顺序才能分包

## 网络分层

- 应用层
  - HTTP
- 传输层
  - tcp/udp 
    - TCP - I/O
      - 保证顺序
      - 不丢失
  - 包 package
- 网络层
  - ip
  - Fragment - 片
- 数据链路层
  - 数字信号
  - 网卡进行物理信号/数字信号转换
    - A/D
    - D/A
  - Frame 帧，没有“连接“的概念
- 物理层
  - 光电信号

> - 中间三层由操作系统实现

## 提高TCP效率

- 调优
- 不使用TCP
  - quic等
  - 在UDP的基础上实现
    - UDP没有拥塞控制
      - 适合大量传输
    - 实时性比较强
    - 不保证顺序

## 面试题

- listen中的backlog参数的意义？
  - 