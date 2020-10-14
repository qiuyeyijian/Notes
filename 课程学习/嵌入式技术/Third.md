# 第三次学习札记



## C#语言基础



### .net 的生态系统

![406794-20170118100517937-1155598716](assets/Thrid/406794-20170118100517937-1155598716.png)

#### Net Framework

Net Framework 是Net的一种实现，在此类库上我们可以使用C#，VB，F#进行程序编写，主要用于构建Windows 下的应用程序

有两部分组成部分：

 i.   公共语言运行时（CLR）处理应用程序

ii.   基础类库（BCL）这是可重用的代码库，使用其编写进行应用程序编写

在执行的过程中.Net编写的代码将会编译成一种称为中间语音（IL）存储形式以DLL和EXE后缀名结尾的文件为主，当程序运行时CLR会编译转换为机器代码。Net Framework 本身不是跨平台的也就是话说仅限于运行在Windows 平台，想要跨平台需要借助第三方。

#### Net Core 是什么

Net Core 的出现就是为了适应软件开发的趋势，因为各种不同的设备还有云计算的出现，其他的操作系统使用量也有所增加，如果Net 不发生改变也就意味着市场将会越来越小。Net Core的出现用于满足当前以及未来软件开发的需求

NetCore是一个全新的框架，是.Net的跨平台的实现，它和Net Framework有很多共同的特性，这也就意味着Net Framework从业者转到Net Core 将会变的很简单。

Net Core的所有方面都是开源的，无论是类库，运行时，编译器。NET Core3.0之后支持了C#，VB，F#。

#### Net Standard 是什么，

Net Standard 是一个规范，它定义了Net Framewoek和Net Core必须实现的Api,它的出现为各种平台上开发的.Net人员解决了代码共享问题，但是仅用于开发类库，意思就是说如果你的类库是Net Standard规范的，那么此类库既可以是Net Framewoek也可以是Net Core类库。

 



缩写

| 缩写 | 全称                                          |
| ---- | --------------------------------------------- |
| SCI  | Serial Communications Interface，串行通信接口 |
|      |                                               |
|      |                                               |

