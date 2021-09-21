# Cisco

## 知识准备

### 三种模式

思科设备有三种模式：

* 一般用户模式，标识符 ">"
* 特权模式，标识符 "#"
* 全局配置模式，标识符 "(config)#"

```
Switch>enable		// 由用户模式进入特权模式
Switch#configure terminal		//由特权模式进入全局配置模式
Switch(config)#
```



### 缩写

思科设备和华为设备配置时，都可以将命令缩写，只要当前模式下，有且只有一个命令和你的缩写相匹配，那么你这个缩写就有效。所以没有固定的缩写格式。

比如交换机在特权模式进入全局配置模式时：

```
Switch#co?			// 输入co有三个命令匹配，所以不能用此缩写
configure  connect  copy 

Switch#conf?		// 输入conf，有且只有一个匹配，可以用此缩写
configure

Switch#conf t		// configure 后面只有一个terminal，所以简写为t就行
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#
Switch(config)#
```



### 禁止DNS解析

如果输入的命令错误，思科设备会认为用户数据的是个域名，就会进行DNS域名解析，但是设备内有没有对应的配置，就会解析大概一分钟后返回错误，此期间用户不能进行其他操作。如果不小心输入错误命令，将会浪费许多时间。我们可以禁止DNS解析来禁止此功能。下面是路由器配置，交换机同理。

```
Router(config)#no ip domain-lookup
```

### 设置超时时间

我们可以设置多少时间没有操作，必须重新登录来保证一定的安全。

```
Router(config)#exec-timeout 10			// 设置超时时间，单位分钟
```



### 批量配置端口

```
SWA(config)#int range f0/1 - 5	// 批量配置接口f0/1 到 fa0/5
```



### 特权模式下的show

```
show vlan brief				// 查看vlan信息
show ip interface brief		// 
show interface fa0/1		// 查看接口
show ip route		// 显示路由信息
show ip protocols	// 显示协议信息
show ip ospf database	// 显示ospf状态数据库
show ip nat translations  // 查看NAT转换记录
show access-list 1			// 查看编号为1的ACL控制列表
```



```
Switch#show int fa0/1
FastEthernet0/1 is up, line protocol is up (connected)
  Hardware is Lance, address is 0002.1746.dd01 (bia 0002.1746.dd01)
 BW 100000 Kbit, DLY 1000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s
  input flow-control is off, output flow-control is off
  ARP type: ARPA, ARP Timeout 04:00:00
```

> 前一个UP是物理层，如果为down，则说明物理线路有问题
>
> 后一个UP是数据链路层，如果为down，则说明协议有问题





### 端口安全



#### （1）设置Console口密码

```
Switch(config)#line console 0
Switch(config)#password yijian		// 设置密码为: yijian
Switch(config)# exit
```

#### （2）设置远程登录telnet密码

首先设置一个VLAN

```
Switch(config)#vlan 20
Switch(config-vlan)#exit
Switch(config)#int vlan 20
Switch(config-if)#ip add 192.168.1.4 255.255.255.0
Switch(config-if)#no shut
```
将与PC机相联的以太网接口划分到改VLAN下

```
Switch(config)#int fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#no shut

```

设置虚拟终端的密码，并启用密码验证

```
RA(config)#line vty 0 4 	//进入路由器线路配置模式
RA(config‐line)#password yijian 		//设置远程登录密码为“yijian” 
RA(config‐line)#login 		//启用密码验证
RA(config‐line)#end
```

最后，将PC机IP地址设置与VLAN在同一网段下，然后使用远程登录命令`telnet`

```
telent 192.168.1.4
```



#### （3）设置enable使能密码

由用户模式进入到特权模式需要使用enable命令，我们可以对用户尝试使用enable命名进入特权模式设置密码，保证设备安全。

设置的密码在配置文件内有两种保存方式： 一种是加密保存，一种是明文保存。

```
RA(config)#enable secret yijian 		//设置路由器特权模式，密文密码为“yijian”
RA(config)#enable password yijian 	   //设置路由器特权模式，明文密码为“yijian”

```



## 交换机

**交换机端口默认是开启的，一般配置端口后，不需要打开命令，但以防万一最好也使用`no shutdown`打开一下，这样可以和路由器配置方式相同，减轻记忆**

**VLAN中有以下两种链路类型：**

> 接入链路（Access Link ）：用于连接用户主机和交换机的链路。通常情况下，主机并不需要知道自己属于哪个 VLAN ，主机硬件通常也不能识别带有 VLAN 标记的帧。因此主机发送和接收的帧是 不带标记 帧。
>
> 干道链路（Trunk Link ）：通常 用于交换机间的连接。干道链路可以承载多个不同 VLAN 数据，数据帧在干道链路传输时，干道链路的两端设备需要能够识别数据帧属于哪个 VLAN ，所以在干道链路上传输的帧 通常 是 带标记 帧 。

**VLAN中的接口类型**

在802.1Q 中定义 VLAN 后，设备的有些接口可以识别 VLAN 帧，有些接口不能识别 VLAN帧。根据对 VLAN 帧的识别情况，将接口分为 4 类：

**（1）Access 接口**

**Access接口是交换机上用来连接用户主机的接口，它只能连接接入链路。仅允许唯一的 VLAN**
**ID 通过本接口**，这个 VLAN ID 与接口的缺省 VLAN ID 相同， Access 接口发往对端设备的以太网帧永远是不带标记的帧。

**（2）Trunk 接口**

**Trunk接口是交换机上用来和其他交换机连接的接口，它只能连接干道链路**，允许多个 VLAN
的帧（带 VLAN 标记）通过。

**（3）Hybrid 接口**

**Hybrid接口是交换机上既可以连接用户主机，又可以连接其他交换机的接口。** Hybrid 接口既
可以连接接入链路又可以连接干道链路。 Hybrid 接口允许多个 VLAN 的帧通过，并可以在出接口方向将某些 VLAN 帧的标记剥掉。

**（4）QinQ 接口**

QinQ（802.1Q in 802.1Q ）接口是使用 QinQ 协议的接口。 QinQ 接口可以给帧加上双重 VLAN标记，即在原来标记的基础上，给帧加上一个新的标记，从而可以支持多达 4094 × 4094 个 VLAN（不同的产品支持不同的规格），满足网络对 VLAN 数量的需求。

### 1. 创建VLAN

`access`一般是主机和交换机的工作模式

`trunk`一般是交换机和交换机的工作模式

#### 设置access模式

此模式场景大概是交换机通过接口来划分VLAN，那些接口主机应用了相同VLAN，那些接口主机就在统一VLAN内，这些主机就可以互相通信。所以说`access`一般是主机和交换机的工作模式

```
SWA(config)#vlan 2		// 创建vlan2
SWA(config-vlan)#name aa		// 可以给vlan起个名字，命名为aa

SWA(config)#int f0/5	// 进入接口f0/5
SWA(config-if)#switchport mode access		// 修改模式为access模式
SWA(config-if)#switchport access vlan 2		// 应用vlan2
SWA(config-if)#exit
```



#### 设置trunk模式

此模式一般在VLAN跨交换机通信时会用到，大概是两台交换机通过以太网接口相连，这两个接口配置成trunk模式，然后配置允许那些VLAN通过。所以说`trunk`一般是交换机和交换机的工作模式

```
SWA(config)#int f0/5	// 进入接口f0/5
SWA(config-if)#switchport mode trunk		// 修改模式为trunk模式
SWA(config-if)#switchport trunk allowed vlan 2		// 允许vlan2通过
SWA(config-if)#switchport trunk allowed vlan all	// 允许所有vlan通过
SWA(config-if)#exit
```



## 路由器

**路由器端口默认是关闭的，所以配置端口后，一般使用`no shutdown`打开**

### 1. 配置端口IP地址

```
RA(config)#interface fastethernet 0/0 	//进入路由器接口配置模式
RA(config‐if)#ip address 192.168.1.1 255.255.255.0 	//配置路由器管理接口IP地址
RA(config‐if)#no shutdown 	//开启路由器fastethernet 0/0接口
RA(config‐if)#exit 	//退出接口配置模式
```



配置串口IP地址

```
RA(config)#interface serial 1/0 //要手动添加模块，并确定是数据通信设备（DCE）还是数据终端设备（DTE）
RA(config‐if)# ip address 172.159.1.1 255.255.255.0
RA(config‐if)#clock rate 64000 //如果是DCE接口，必须配时钟频率；另一头自动适应为DTE接口，不需要配时钟频率；
RA(config‐if)#no shutdown
RA(config‐if)#end
RA#show IP interface brief //验证各接口的IP地址已经配置和开启
```



### 3. 配置静态路由

```
Router(config)#ip route 192.168.3.0 255.255.255.0 192.168.2.2
Router(config)#end
```



### 4. 配置RIP

```
Router(config)#router rip
Router(config)#version 2
Router(config)#no auto-summary
Router(config‐router)#network 192.168.1.0
Router(config‐router)#network 192.168.2.0
Router(config‐router)#end


Router(config)#no router rip		// 删除RIP
```



### 5. 配置NAT

```
// 配置标准访问列表，定义可被转换的私有地址
Router(config)#access-list 1 permit 192.168.10.0 0.0.0.255

// 定义公网地址池
Router(config)#ip nat pool compool 98.5.5.100 98.5.5.200 netmask 255.255.255.0

// 为服务器定义一个可静态转换的公网地址,就是边界路由器的外网地址
Router(config)#ip nat inside source static 192.168.10.12 98.5.5.1

// 将地址池与访问列表相关联
Router(config)#ip nat inside source list 1 pool compool overload
// 如果只有一个公网地址，可以不用设置地址池，将公网接口与访问控制列表相关联
Router(config)#ip nat inside source list 1 int s1/0 overload

// 定义内网接口
Router(config)#int f0/0
Router(config-if)#ip nat inside

// 定义外网接口
Router(config)#int s1/0
Router(config-if)#ip nat outside

```



### 6. 配置OSPF

OSPF和ACL要使用反掩码

```
Router(config)#router ospf 2		// router ospf 进程号
Router(config)#network 192.168.1.0 0.0.0.255 area 1		// 发布子网要加区域号
```



### 7. 配置独臂路由

配置交换机

```
SW(config-if)#int fa0/1
SW(config-if)#switchport mode access
SW(config-if)#switchport access vlan 10
SW(config-if)#int fa0/2
SW(config-if)#switchport mode access
SW(config-if)#switchport access vlan 20
SW(config-if)#int g0/1		//与路由器相连的接口
SW(config-if)#switchport mode trunk
SW(config-if)#switchport trunk allowed vlan all

```



配置路由器

```
Router(config)#int fa0/0.1		// 在fa0/0 中创建一个虚拟接口0.1
Router(config)#encapsulation dot1Q 10			// 后面的10是连接的交换机划分的VLAN号
Router(config)#ip addr 192.168.10.1 255.255.255.0
Router(config)#no shut
Router(config)#
Router(config)#int fa0/0.2		// 在fa0/0 中创建一个虚拟接口0.2
Router(config)#encapsulation dot1Q 20			// 后面的100是连接的交换机划分的VLAN号
Router(config)#ip addr 192.168.20.1 255.255.255.0
Router(config)#no shut
Router(config)#
Router(config)#int fa0/0	
Router(config)#no shut
Router(config)#
```



### 6. 配置接口

路由器通过同步串口连接，提供时钟的叫做DCT，另外一是终端路由叫做DTE

```
clock rate
```



### 7. 配置ACL

ACL分为两种：标准访问控制列表，编号1-99,1300-1999。扩展访问控制列表，编号100-199,2000-2699。ACL使用的是反掩码

配置ACL需要实施两个步骤

* 创建ACL
* 在接口上应用ACL

```
Router(config)#access-list 1 permit 172.16.1.0 0.0.0.255
Router(config)#int f0/1
Router(config)#ip access-group 1 out
Router(config)#
```

