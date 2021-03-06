# 计算机网络

## 性能指标

时延、RTT、时延带宽积、带宽

三种通信方式：单工通信、半双工通信、全双工通信

数据传输方式：串行、并行

## TCP/IP参考模型

### 物理层

传输介质：双绞线、同轴电缆、光纤、无线电波、卫星

### 数据链路层

将物理层可能出现的物理连接改造为逻辑上无差错的数据链路，使之对网络层表现为一条无差错的链路。

**功能**：为网络层提供服务、链路管理：即连接的建立、维持和释放（用于面向连接的服务）、组帧、流量控制、差错控制

**差错控制**：检错编码：：奇偶校验码、循环冗余码，纠错编码：：海明码

**流量控制与可靠机制传输**：

数据链路层的流量控制是点对点的，接收方不回复就不回复确认，而传输层的流量控制是端对端的，接收端会给发送端一个窗口公告。

停止等待协议

滑动窗口协议：

​	后退N帧协议（GBN）：连续发送帧提高了信道利用率，但是在重传时会把已经正确传送的数据帧重传，效率降低。

​	选择重传协议（SR）：对数据帧逐一确认，只重传出错帧，接收方有缓存。

**介质访问控制**：在广播链路中，访问共享信道时发生冲突

**PPP协议**、**HDLC协议**

### 网络层

**功能**：路由选择和分组转发、拥塞控制

**数据交换方式**：电路交换、报文交换、分组交换

**路由算法与路由协议**

**IP数据报格式**：

![](http://c.biancheng.net/uploads/allimg/191106/6-191106153044K1.gif)

协议：数据部分的协议值，TCP对应的是6，UDP对应的是17

**分类的IP地址**

**网络地址转换NAT**:在专用网连接到因特网的路由器上安装NAT软件

**子网掩码与子网划分**

IP地址与子网掩码做与运算，得到网络地址。

**无分类域间路由选择CIDR：**如128.14.32.0/20

**ARP协议**:地址解析协议

**DHCP**

**ICMP**

### 传输层

**TCP三次握手和四次分手**

**TCP可靠传输**：确认、重传机制

**TCP流量控制**：滑动窗口机制来实现流量控制。接受方根据自己接受缓存的大小，动态的调整发送方的发送窗口的大小。

**TCP拥塞控制**:慢开始拥塞避免、快重传快恢复

### 应用层

文件传送协议：FTP

电子邮件：SMTP、POP3

查询服务：DNS























































