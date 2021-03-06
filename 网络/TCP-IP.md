# 一、概述

## 1.1 发展过程

* 主机带终端形式的计算机网络（20世纪50年代）
  * 一个中心携带多个终端的网络体系
* 基于通信网的计算机网络（20世纪60年代中期—70年代中期）
  * 利用一个分组交换机连接多个终端
* 标准化的计算机网络（20世纪70年代中期—90年代初期）
  * 国际化标准组织(ISO)提出OSI(开放式系统互联参考模型)标准
  * 1980年，IEEE 802委员会制定了局域网标准
* 以Internet为代表的计算机互联网络（20世纪90年代起）
  * 不同的网络体系通过路由连接到一起

## 1.2 分类

### 1.2.1 按照覆盖范围

* 局域网（Local Area Network,LAN）
  * 一幢大楼、一个实验室、一所校园
  * 不对外提供服务
  * 分布范围小（10km以内）
  * 组建简单
  * 传输速度快
  * 传输速度快、误码率低
  * 最活跃

* 广域网（Wide Area Network,WAN）
  * 覆盖范围最大（多个城市、多个国家、甚至全球）
  * 网络成本高
  * 安全系数低
  * 传输速度慢、误码率高

* 城域网
  * 讨论较少

### 1.2.2 按照拓扑结构

* 星型网络
* 树型网络
* 总线型网络
* 环型网络
* 网状网络

### 1.2.3 按照交换技术

* 线路交换网
  * 建立真实的物理连接

* 报文交换网
  * 封装要传输的数据为一个报文，然后一次性传输出去
  * 各个传输节点可以将报文缓存，等空闲的时候再传输
  * 不存在专用线路

* 分组交换网【重点】
  * 将大的报文分成若干数据块，即分组
  * 待所有分组到达目的主机时，再将所有分组重组为原来的数据
  * 每个分组走的路由线路不一样（数据报方式）
  * 每个分组走的路由线路一样（虚电路方式）

## 1.3 计算机网络体系结构

* 每个分层都接收由它下一层所提供的服务，并且负责为自己的上一层提供特定的服务
* 上下层之间进行交互时所遵循的约定叫做"接口"
* 同一层之间的交互所遵循的约定叫做"协议"

### 1.3.1 OSI参考模型

* 网络互联的7层框架
* 由于众多原因，它仅仅是一个模型，并没有完成相应的协议实现，只提供了一系列描述性的概念
* OSI不是一个标准，而是一个在制定标准时需要使用的概念性框架

* 物理层
  * 传输单元是比特（0和1组成）

* 数据链路层

  * ==互联设备之间的传送和识别数据帧==

  * 传输单位为帧

* 网络层
  * 任务是通过路由算法为分组选择最合适的通信路径，实现网络互联和拥塞控制
  * ==地址管理和路由选择==
  * 传输单元为分组

* 传输层
  * 任务是为用户提供端到端的服务
  * 是通信体系中最关键的一层
  * ==管理两个节点之间的数据传输，确保数据可靠地传送到目的地址==
  * ==起着可靠传输的作用，只在通信双方节点上进行处理，而无需在路由器上进行处理==
  * 实现向高层屏蔽下层数据通信的全部细节

* 会话层
  * 目前该层没有具体的协议
* 表示层
  * 目前该层没有具体的协议

* 应用层
  * 应用层包含了用户通常需要的各种协议

### 1.3.2 TCP/IP参考模型【重点】

* 虽然OSI参考模型理论较为完整，但是并没有具体实现，也没有广泛应用在网络通信中
* 而TCP/IP参考模型在大范围内实现了应用
* 一共出现了6个版本，后三个版本为4、5、6.目前应用最广泛的是版本4（IPv4）
* 一共分为4层
* 网络接口层【数据链路】
  * 用于提供对下面的数据链路层和物理层的接口
  * 各种硬件的驱动程序就属于这一层

* 网络层【关键】
  * 对应OSI中的网络层
  * 主要功能是负责将源主机的报文分组独立地发送到目的主机
  * IP

* 传输层
  * 对应OSI中的传输层和会话层
  * 主要功能对应用层数据进行分段，对接收数据进行检查保证数据完整性
  * 为多个应用进程同时传输数据
  * ==定义了两个协议：TCP（传输控制协议）和UDP（用户数据报协议）==

* 应用层
  * 对应OSI中的应用层
  * 为用户提供常用的应用程序，并规定各种应用程序之间通过什么样的应用协议来使用网络所提供的服务
  * 包含所有的高层协议：HTTP、Telnet、FTP、SMTP、DNS等

## 1.4 地址

* 在实际的网络通信中，每一层的协议所使用的地址都不尽相同
  * TCP/IP中使用MAC地址、IP地址、端口号
  * 甚至在应用层中，可将电子邮件地址作为网络通信的地址

### 1.4.1 地址的唯一性

* 在同一个通信网络中不允许有两个相同地址的通信主体存在

### 1.4.2 地址的层次性

* IP地址和MAC地址在标识一个通信主体时虽然都具有唯一性
  * MAC地址由制造商识别号、制造商内部产品编号和产品通用编号确保MAC的唯一性
* 但是==只有IP地址具有层次性==
  * IP地址由网络号和主机号两部分组成

## 1.5 网络组成硬件设备

### 1.9.1 通信媒介和数据链路

* 双绞线电缆
* 光纤电缆
* 同轴电缆
* 串行电缆

### 1.9.2 网卡

* 任何一台计算机连接网络时，必须要使用网卡（NIC，也称网络适配器、LAN卡）

### 1.9.3 中继器

* OSI第一层—物理层中的设备
* 对信号进行再放大

### 1.9.4 网桥-OSI第二层设备【数据链路层】

* 比如交换集线器

### 1.9.5 路由器-OSI第三层设备【网络层】

* 连接不同的网络

### 1.9.6 OSI4~7层设备

* 比如负载均衡器
* 广域网加速器
* 防火墙

### 1.9.7 网关

* 负责不同通信协议的转换



# 二、数据链路

## 2.1 主要数据链路

* 以太网【使用最广】
* 802.11
* Bluetooth
* ATM
* POS
* FDDI
* Token Ring
* HIPPI

# 三、网络层

## 3.1 IP协议

### 3.1.1 网络层与数据链路层的关系

* 数据链路层提供==直连==两个设备之间的通信功能
* 网络层负责在没有直连的两个网络之间进行通信传输
* 直连—是指属于同一类型的数据链路中的设备（比如同是以太网）

### 3.1.2 IP基础知识

* IP地址属于网络层地址
* 路由控制是指将分组数据发送到最终目的地址的功能
* IP属于面向无连接型
  * ==即在发送IP包之前，不需要建立对端目标地址之间的连接==
  * ==即使目的主机关机或者不存在，数据包还是会被发送出去==
  * 好处是简化和提速
  * ==弥补IP的缺点由它的上一层来完善==

### 3.1.3 IP地址

* 全局IP（公网IP）在世界范围类都是唯一的
* 私有IP只需要保证在自己的网络内唯一即可

## 3.2 IPv4首部

包括：

* 版本
* 首部长度
* 区分服务（TOS、DSCP、ECN）
* 总长度（IP首部+数据，最大为65536字节）
* 标识
* 标志
* 片偏移
* 生存时间
* 协议
* 首部校验和
* 源地址（发送端IP地址）
* 目标地址（接收端IP地址）
* 可选字段
* 填充
* 数据

## 3.3 IP协议相关技术

### 3.3.1 DNS

* IP地址不方便记忆
* 主机中一个叫hosts的数据库文件
* 需要架设DNS服务器来进行管理域名

### 3.3.2 ARP

* 以IP地址为线索，用来定位下一个应该接收数据包的网络设备的对于的MAC地址
* 需要架设ARP服务器

### 3.3.3 RARP

* 从MAC地址定位IP地址的一种协议
* 需要架设RARP服务器

### 3.3.4 ICMP

* 确认IP包是否成功送到目的地址
* 通知在发送过程中IP包被废弃的原因
* 改善网络设置

### 3.3.5 DHCP

* 实现自动为网络设备设置IP
* 统一管理IP地址分配
* 需要架设DHCP服务器来进行管理这些IP

### 3.3.6 NAT

* 主要作用转换IP
* 可以将本地网络中的私有IP转换为全局IP进行通信
* 需要使用NAT路由器
* NAT-PT路由器（IPv4首部---转为---IPv6首部）



# 四、传输层

## 4.1 传输层的作用

* ==IP层只管把数据发送到指定的IP的主机，而不能确定发给主机中具体哪一个应用程序==
* 传输层是可以指定发给哪一个应用程序的

## 4.2 端口号

* 在数据链路层中用MAC地址来识别同一链路中不同的计算机
* 在网络层中用IP地址来识别主机和路由器
* ==而在传输层中则用端口号来识别同一台主机中不用的应用程序==
* 区别一个通信是不是同一个，可以通过下面5个信息来进行识别
  * 源IP地址
  * 目标IP地址
  * 协议号
  * 源端口号
  * 目标端口号

* 只要其中有一项不同，则两个通信就不是同一个

### 4.2.1 知名端口号和相应的协议

* 该部分是计算机自带的，不应该修改。范围0到1023
  * 端口80     HTTP服务
  * 端口443   HTTPS服务
  * 端口22     SSH远程登录服务

## 4.3 UDP

* UDP是不具有可靠性的数据报协议，虽然可以确保发送信息的大小，却不能保证信息一定会到达
* UDP不提供复杂的控制机制，利用IP提供==面向无连接==的通信服务，接收到数据就按照原样发送
* 即使网络出现拥堵，UDP也无法进行流量控制和避免网络拥塞的行为
* 传输过程中出现丢包，也不负责重发
* 常用于：
  * 包总量较少的通信（DNS、SNMP等）
  * 视频、音频等多媒体通信
  * 广播通信（广播、多播）

### 4.3.1 UDP首部格式

* 源端口号
* 目标端口号
* 包长度
* 校验和

## 4.4 TCP ++

* TCP相比于UDP加入了对传输的许多控制
* TCP是一种==面向有连接==的协议，只有确认通信对端存在时才会发送数据，从而避免通信流量浪费

* 对传输的各种控制功能
  * 丢包时进行重发
  * 对次序乱掉的包进行顺序控制

* 总之，TCP负责==连接的建立、断开和保持==等管理工作

### 4.4.1 通过包的序列号与确认应答提高可靠性

* 在TCP中，当发送端的数据到达接收主机时，接收端主机会返回一个已收到消息的通知，这个消息叫做确认应答（ACK，Positive Acknowledgement）,否定确认应答为NACK（Negative Acknowledgement）

* 确认应答信息里面包括下一步应该接收的包序号

  <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200108150717619.png" alt="image-20200108150717619" style="zoom:50%;" />

* 在一定的时间内没有收到确认应答，发送端就可以认为数据已经丢失，并进行重发

  <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200108150451695.png" alt="image-20200108150451695" style="zoom: 50%;" />

* 未收到确认信息并不意味着数据一定丢失，可能数据已经收到，但是确认收到信息在返回的途中丢失，这种情况也会进行重发

  <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200108150943606.png" alt="image-20200108150943606" style="zoom:50%;" />

* ==基于此，对于接受端来说，它必须得丢弃重复的数据才行==



### 4.4.2 重发超时如何确定

* 重发超时的时间每次都会计算
* 这个时间为每次发包的往返时间（因为还要算上确认应答回来的时间）（RTT）加上偏差
* 如果达到一定的重发次数仍然收不到确认信息，就会认为对端主机发生了异常，强制关闭连接



### 4.4.3 连接管理++

* TCP是面向连接的，即在数据通信发生之前要先做好通信两端的准备工作，UDP则不会这么做

  * TCP在数据通信之前，会发送一个SYN包（同步序列编号， Synchronize Sequence Numbers）作为建立连接的请求，然后等待应答。

  * 接收端发送确认应答（ACK）

  * 发送端发送确认应答（ACK）

  * 当这三步（三次握手）全部完成后，这认为可以进行数据通信

    <img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200108153513013.png" alt="image-20200108153513013" style="zoom:50%;" />

  * ==当断开连接的时候==，接收端和发送端都会发送FIN包给对方（四次握手切断连接）

### 4.4.4 窗口控制和相应的重发控制

* 如果每发送一个TCP包（段）都要等到确认信息后才发送下一个段，就会降低通信的性能
* 为此引入窗口，窗口就是包含了多个段，然后把这个窗口中的段依次发送出去，而不需要等待确认信息
* 利用窗口可以提高传输的速度

### 4.4.5 拥塞控制

* TCP会根据接收端主机可以接受的窗口的大小和网络带宽的情况来动态地控制发送窗口的大小

### 4.4.6 TCP首部格式

* 源端口号
* 目标端口号
* 序列号（序列号不会从0或者1开始，二是计算机生成的随机数）
* 确认应答号
* 数据偏移
* 保留
* 控制位
  * ACK
  * SYN
  * FIN
  * 等等
* 窗口大小
* 校验和

# 五、应用层

## 5.1 应用层协议概况

* 应用协议是为了实现某种应用而设计和创造的协议
* TCP\IP等下层协议是不依赖于上层的应用类型、适用性非常广的协议
* 应用层协议可以由设计师和开发人员根据所开发模块的功能和目的，自己定义一个新的应用协议



## 5.2 远程登录

主要协议有：

* TELNET——端口23
* SSH

## 5.3 文件传输

* ftp——端口21

## 5.4 电子邮件

* SMTP
* POP
* IMAP

## 5.5 WWW

* HTTP—端口80



