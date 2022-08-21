# 应用层 (Application)
<br>

## 作用

在两节点间建立和维持通信，通过主机间的交互来**支持各种网络应用**，应用层的协议就是进程间通信的协议

## 工作设备 

应用程序网关(application gateway)

## 协议

- Telnet:远程登陆
- FTP (File Transfer Protocol)：文件传输协议
- HTTP (Hyper Text Transfer Protocol)：超文本传输协议
- SMTP (Simple Mail Transter Protocol)：简单邮件传输协议
- POP3 (Post Office Protocol)：邮局协议
- SNMP (Simple Network Manage)： 简单网络管理协议
- DNS (Domain Name System)： 域名系统 

---
# 传输层 (Transport)
<br>

## 作用

传输层负责 源 到 目的（端到端） 的 **进程间完整报文的传输**

端到端的概念见 [[端到端  &  点到点]]

## 工作设备

传输网关 (transport gateway)

## 协议

- TCP (Transmission Control Protocol)： 传输控制协议
- UDP (User Data Protocol)： 用户数据协议

TCP 和 UDP 的概念与比较见 [[TCP 相关知识点]]

---
# 网络层（Internet）
<br>

## 作用

负责 **源主机** 到 **目的主机** 数据分组 packet 的路由和转发

## 工作设备

多协议路由器 (multiprotocol router)

## 协议

- IP (Internet Protocol) ：网络协议
- ARP (Address Resolution Protocol)：地址解析协议
- RARP (Reverse Address Resolution Protocol)：逆地址解析协议
- ICMP (Internet Control Message Protocol)：因特网控制消息协议
- IGMP (Internet Group Manage Protocol)：因特网组管理协议
- BOOTP (Bootstrap Protocol)： 可选安全启动协议

---
# 数据链路层（Data Link）
<br>

## 作用

实现节点（主机、交换机、路由器）间的数据传输

## 工作设备

网桥（bridge） 交换机（switcher）

## 协议

- HDLC (High Data Link Control)：高级数据链路控制 
- SLIP (Serial Line IP)： 串行线路IP
- PPP (Point - to - Point Protocol) ： 点到点协议

---
# 物理层（Physical）
<br>

## 作用

实现比特流的传输

## 工作设备

中继器（repeater） 集线器（hub）

## 协议

无





