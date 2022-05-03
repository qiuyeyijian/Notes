# 套接字通信类的封装

## 基于C语言的封装

基于 TCP 的套接字通信分为两部分：服务器端通信和客户端通信。我们只要掌握了通信流程，封装出对应的功能函数也就不在话下了，先来回顾一下通信流程：

服务器端

- 创建用于监听的套接字
- 将用于监听的套接字和本地的 IP 以及端口进行绑定
- 启动监听
- 等待并接受新的客户端连接，连接建立得到用于通信的套接字和客户端的 IP、端口信息
- 使用得到的通信的套接字和客户端通信（接收和发送数据）
- 通信结束，关闭套接字（监听 + 通信）

客户端

- 创建用于通信的套接字
- 使用服务器端绑定的 IP 和端口连接服务器
- 使用通信的套接字和服务器通信（发送和接收数据）
- 通信结束，关闭套接字（通信）

```cpp
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <unistd.h>

///////////////////////////////////////////////////
//////////////////// 服务器 ///////////////////////
///////////////////////////////////////////////////
int bindSocket(int lfd, unsigned short port);
int setListen(int lfd);
int acceptConn(int lfd, struct sockaddr_in* addr);

///////////////////////////////////////////////////
//////////////////// 客户端 ///////////////////////
///////////////////////////////////////////////////
int connectToHost(int fd, const char* ip, unsigned short port);

///////////////////////////////////////////////////
///////////////////// 共用 ////////////////////////
///////////////////////////////////////////////////
int createSocket();
int sendMsg(int fd, const char* msg);
int recvMsg(int fd, char* msg, int size);
int closeSocket(int fd);
int readn(int fd, char* buf, int size);
int writen(int fd, const char* msg, int size);

// 创建监套接字
int createSocket() {
  int fd = socket(AF_INET, SOCK_STREAM, 0);
  if (fd == -1) {
    perror("socket");
    return -1;
  }
  printf("套接字创建成功, fd=%d\n", fd);
  return fd;
}

// 绑定本地的IP和端口
int bindSocket(int lfd, unsigned short port) {
  struct sockaddr_in saddr;
  saddr.sin_family = AF_INET;
  saddr.sin_port = htons(port);
  saddr.sin_addr.s_addr = INADDR_ANY;  // 0 = 0.0.0.0
  int ret = bind(lfd, (struct sockaddr*)&saddr, sizeof(saddr));
  if (ret == -1) {
    perror("bind");
    return -1;
  }
  printf("套接字绑定成功, ip: %s, port: %d\n", inet_ntoa(saddr.sin_addr), port);
  return ret;
}

// 设置监听
int setListen(int lfd) {
  int ret = listen(lfd, 128);
  if (ret == -1) {
    perror("listen");
    return -1;
  }
  printf("设置监听成功...\n");
  return ret;
}

// 阻塞并等待客户端的连接
int acceptConn(int lfd, struct sockaddr_in* addr) {
  int cfd = -1;
  if (addr == NULL) {
    cfd = accept(lfd, NULL, NULL);
  } else {
    socklen_t addrlen = sizeof(struct sockaddr_in);
    cfd = accept(lfd, (struct sockaddr*)addr, &addrlen);
  }
  if (cfd == -1) {
    perror("accept");
    return -1;
  }
  printf("成功和客户端建立连接...\n");
  return cfd;
}

// 接收数据
int recvMsg(int cfd, char** msg) {
  if (msg == NULL || cfd <= 0) {
    return -1;
  }
  // 接收数据
  // 1. 读数据头
  int len = 0;
  readn(cfd, (char*)&len, 4);
  len = ntohl(len);
  printf("数据块大小: %d\n", len);

  // 根据读出的长度分配内存
  char* buf = (char*)malloc(len + 1);
  int ret = readn(cfd, buf, len);
  if (ret != len) {
    return -1;
  }
  buf[len] = '\0';
  *msg = buf;

  return ret;
}

// 发送数据
int sendMsg(int cfd, char* msg, int len) {
  if (msg == NULL || len <= 0) {
    return -1;
  }
  // 申请内存空间: 数据长度 + 包头4字节(存储数据长度)
  char* data = (char*)malloc(len + 4);
  int bigLen = htonl(len);
  memcpy(data, &bigLen, 4);
  memcpy(data + 4, msg, len);
  // 发送数据
  int ret = writen(cfd, data, len + 4);
  return ret;
}

// 连接服务器
int connectToHost(int fd, const char* ip, unsigned short port) {
  // 2. 连接服务器IP port
  struct sockaddr_in saddr;
  saddr.sin_family = AF_INET;
  saddr.sin_port = htons(port);
  inet_pton(AF_INET, ip, &saddr.sin_addr.s_addr);
  int ret = connect(fd, (struct sockaddr*)&saddr, sizeof(saddr));
  if (ret == -1) {
    perror("connect");
    return -1;
  }
  printf("成功和服务器建立连接...\n");
  return ret;
}

// 关闭套接字
int closeSocket(int fd) {
  int ret = close(fd);
  if (ret == -1) {
    perror("close");
  }
  return ret;
}

// 接收指定的字节数
// 函数调用成功返回 size
int readn(int fd, char* buf, int size) {
  int nread = 0;
  int left = size;
  char* p = buf;

  while (left > 0) {
    if ((nread = read(fd, p, left)) > 0) {
      p += nread;
      left -= nread;
    } else if (nread == -1) {
      return -1;
    }
  }
  return size;
}

// 发送指定的字节数
// 函数调用成功返回 size
int writen(int fd, const char* msg, int size) {
  int left = size;
  int nwrite = 0;
  const char* p = msg;

  while (left > 0) {
    if ((nwrite = write(fd, msg, left)) > 0) {
      p += nwrite;
      left -= nwrite;
    } else if (nwrite == -1) {
      return -1;
    }
  }
  return size;
}
```



## 基于C++的封装

编写 C++ 程序应当遵循面向对象三要素：封装、继承、多态。简单地说就是封装之后的类可以隐藏掉某些属性使操作更简单并且类的功能要单一，如果要代码重用可以进行类之间的继承，如果要让函数的使用更加灵活可以使用多态。因此，我们需要封装两个类：客户端类和服务器端的类。

根据面向对象的思想，整个通信过程不管是监听还是通信的套接字都是可以封装到类的内部并且将其隐藏掉，这样相关操作函数的参数也就随之减少了，使用者用起来也更简便。



通过对客户端的操作进行封装，我们可以看到有如下的变化：

文件描述被隐藏了，封装到了类的内部已经无法进行外部访问

功能函数的参数变少了，因为类成员函数可以直接使用类内部的成员变量。

创建和销毁套接字的函数去掉了，这两个操作可以分别放到构造和析构函数内部进行处理。

在 C++ 中可以适当的将 char* 替换为 string 类，这样操作字符串就更简便一些。



通过对服务器端的操作进行封装，我们可以看到这个类和客户端的类结构以及封装思路是差不多的，并且两个类的内部有些操作的重叠的：接收和发送通信数据的函数 recvMsg()、sendMsg()，以及内部函数 readn()、writen()。不仅如此服务器端的类设计成这样样子是有缺陷的：服务器端一般需要和多个客户端建立连接，因此通信的套接字就需要有 N 个，但是在上面封装的类里边只有一个。

既然如此，我们如何解决服务器和客户端的代码冗余和服务器不能跟多客户端通信的问题呢？

答：瘦身、减负。可以将服务器的通信功能去掉，只留下监听并建立新连接一个功能。将客户端类变成一个专门用于套接字通信的类即可。服务器端整个流程使用服务器类 + 通信类来处理；客户端整个流程通过通信的类来处理。



其中无参构造一般在客户端使用，通过这个套接字对象再和服务器进行连接，之后就可以通信了

有参构造主要在服务器端使用，当服务器端得到了一个用于通信的套接字对象之后，就可以基于这个套接字直接通信，因此不需要再次进行连接操作。



## 测试代码

### 客户端





































































































































