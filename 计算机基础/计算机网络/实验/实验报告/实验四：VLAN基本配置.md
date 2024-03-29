## 一、实验预习报告

### 1、实验相关知识简述

VLAN（Virtual Local Area Network）是一种将局域网设备从逻辑上划分成多个网段，从而实现虚拟工作组的数据交换技术。目前常用的VLAN划分方式为**基于端口的划分和基于MAC地址划分**。

基于端口的划分就是将交换机上的物理端口分成若干个组，每个组构成一个虚拟网，相当于一个独立的交换机，如果不同VLAN之间需要互访时，可通过路由协议转发。下面主要介绍基于端口的VLAN配置方法。

### 2、实验原理预习情况

了解了VLAN的链路类型和接口类型。

熟悉VLAN划分的主要方法。

学习VLAN的工作原理和帧格式。

### 3、实验注意事项

注意Cisco用户模式、特权模式、全局配置模式分别对应的命令。

注意禁止IP地址解析特性，避免因为输入错误命令，浪费不必要的时间



## 二、实验报告

### 1、实验目的与要求

通过对交换机VLAN配置实验，加深对相关交换机VLAN工作原理理解，掌握基于交换机端口的VLAN的配置方法，为将来从事网络工程建设打下基础。

### 2、实验仪器或材料

- 一台装有Windows 10系统的PC机
- Packet Tracker软件



### 3、实验原理

有时属于同一个VLAN 的用户主机被连接在不同的交换机上。当 VLAN 跨越交换机时，就需要交换机间的接口能够同时识别和发送跨越交换机的 VLAN 报文。这时，需要用到 Trunk 技术。 

Trunk有两个作用：

* 中继作用：把 VL AN 报文透传到互联的交换机。
* 干线作用：一条 Trunk Link 上可以传输多个 VLAN 的报文。

### 4、实验过程及数据记录

#### （1）网络拓扑

![image-20201130162239696](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130162239696.png)



**网络拓扑说明**

| 序号 | 设备        | 设备类型   | 规格型号 |
| ---- | ----------- | ---------- | -------- |
| 1    | PC0 ~ PC7   | 用户主机   | PC       |
| 2    | SW-1 ~ SW-2 | 两层交换机 | 2960     |



#### （2）网络规划

**交换机接口与VLAN规划**

| 序号 | 交换机名 | 接口   | 所属VLAN ID | 连接设备 | 接口类型 |
| ---- | -------- | ------ | ----------- | -------- | -------- |
| 1    | SW-1     | Fa 0/1 | 10          | PC0      | Access   |
| 2    | SW-1     | Fa 0/2 | 10          | PC1      | Access   |
| 3    | SW-1     | Fa0/6  | 20          | PC2      | Access   |
| 3    | SW-1     | Fa0/7  | 20          | PC3      | Access   |
| 4    | SW-2     | Fa 0/1 | 10          | PC4      | Access   |
| 5    | SW-2     | Fa 0/2 | 10          | PC5      | Access   |
| 6    | SW-2     | Fa 0/6 | 20          | PC6      | Access   |
| 7    | SW-2     | Fa 0/7 | 20          | PC7      | Access   |
| 8    | SW-1     | G0/1   | all         | SW-2     | Trunk    |
| 9    | SW-2     | G0/1   | all         | SW-1     | Trunk    |



**主机IP地址规划**

| 序号 | 设备名称 | IP地址 / 子网掩码 | 默认网关    | 接入位置     |
| ---- | -------- | ----------------- | ----------- | ------------ |
| 1    | PC0      | 192.168.1.11/24   | 192.168.1.1 | SW-1  Fa 0/1 |
| 2    | PC1      | 192.168.1.12/24   | 192.168.1.1 | SW-1  Fa 0/2 |
| 3    | PC2      | 192.168.1.21/24   | 192.168.1.1 | SW-1  Fa 0/6 |
| 4    | PC3      | 192.168.1.22/24   | 192.168.1.1 | SW-1  Fa 0/7 |
| 5    | PC4      | 192.168.1.13/24   | 192.168.1.1 | SW-2  Fa 0/1 |
| 6    | PC5      | 192.168.1.14/24   | 192.168.1.1 | SW-2  Fa 0/2 |
| 7    | PC6      | 192.168.1.23/24   | 192.168.1.1 | SW-2  Fa 0/6 |
| 8    | PC7      | 192.168.1.24/24   | 192.168.1.1 | SW-2  Fa 0/7 |



#### （3）实验步骤

1. 根据网络规划，搭建网络拓扑，配置主机的IP地址，此处给出PC0的配置图，其他同理

![image-20201130171359157](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130171359157.png)

2. 创建两个VLAN：VLAN10和VLAN20。分别应用到接口Fa0/1、Fa0/2和Fa0/6、Fa0/7。此处给出将Fa0/1划分到VLAN10的方法，其他同理

![image-20201130171746965](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130171746965.png)

3. 将链接两个交换机的接口G0/1配置成Trunk链路，允许所有VLAN通过。此处给出SW-1的配置，SW-2配置同理。

![image-20201130172039244](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130172039244.png)

4. 打开PC0的终端，分别PING同一VLAN下的PC1和PC4发现都能PING通

![image-20201130172309219](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130172309219.png)

但是PING 同一网段不同VLAN下的PC2和PC6，会超时

![image-20201130172500611](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130172500611.png)

![image-20201130172602946](assets/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image-20201130172602946.png)





### 5、实验结果分析

所有主机都在同一网段下：

| VLAN | 交换机 | 能否PING通 |
| ---- | ------ | ---------- |
| 相同 | 相同   | 能         |
| 相同 | 不同   | 能         |
| 不同 | 相同   | 不能       |
| 不同 | 不同   | 不能       |



## 三、实验总结

通过本次实验，我学习到了如何利用VLAN来划分广播域。在实际工作中，我们可以利用VLAN来从逻辑上划分各部门的通信，使得相同部门可以互相通信，不同部门之间不能相互通信。

虽然各个主机处于同一网段，但是经过VLAN划分，可以从逻辑上来划分广播域。如果不同VLAN之间想要通信，可以通过单臂路由或者三层交换机来实现。



