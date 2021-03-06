## OS操作系统名词解释&简答题

### :rocket:  操作系统概念

#### 操作系统

管理系统资源、控制程序执行、改善人机界面、提供各种服务，并合理组织计算机工作流程和为用户方便有效地使用计算机提供良好运行环境的一种系统软件。

#### 多道程序设计

允许多个作业（程序）同时进入计算机系统的内存并启动交替计算的方法。

#### 简述操作系统的资源管理技术

**资源复用**：让众多进程共享物理资源，解决物理资源数量不足

**资源虚化**：对资源进行转化、模拟或整合，把物理上的一个资源变成逻辑上的多个对应物的一类技术

**资源抽象**：复用和虚拟的主要目标是解决物理资源数量不足的问题，抽象则用于处理系统复杂性，重点解决资源易用性。 



#### 操作系统的主要特性

**并发性**：两个或两个以上的活动或事件在同一时间间隔内发生。

**共享性**：系统中的资源可供内存中多个并发执行的进程共同使用

**虚拟性**：在操作系统中，虚拟是指把一个物理上的实体变成若干个逻辑上的对应物，前者是实际存在的，后者是虚拟的，这只是用户的一种感觉。

**异步性**：多道程序环境允许多个程序并发执行，但由于资源有限，进程的执行并不是一贯到底的，而是走走停停，它以不可预知的速度向前推进，这就是进程的异步性。

==异步性使得操作系统运行在一种随机的环境下，可能导致进程产生与时间有关的错误。==

==并行性是指两个或多个事件在同一时刻发生，注意与并行性的区别==



#### 从资源管理的角度来看，操作系统的功能有哪些

**处理器管理**。处理器是计算机系统中最为稀有和宝贵的资源，应该最大限度地提高其利用率。常常采用多道程序设计技术组织多个作业同时执行，解决处理器调度、分配和回收问题。

**存储管理**。存储管理的主要任务是管理内存资源，为多道程序设计提供有力支持，提高存储空间利用率，具体来说有内存分配和回收、地址转换与存储保护、内存共享与存储扩充等。

**设备管理**。设备管理的主要任务是管理各种外部设备，完成用户提出的I/O请求；加快数据传输速度，发挥设备的并行性，提高设备的利用率；提供设备驱动程序和中断处理程序，为用户隐蔽硬件操作细节，提供简单的设备使用方法。

**文件管理**。主要任务是对用户和系统文件进行有效管理，实现按名存取；实现文件共享、保护和保密；保证文件的安全性；向用户提供一整套能够方便地使用文件的操作和命令。

**联网和通信管理**。操作系统至少具有以下与网络相关的功能：网络资源管理、数据通信管理、应用服务、网络管理。



#### 简述实现多道程序设计必须解决的主要问题

**存储保护与程序浮动**。硬件必须提供相应的设施，使得内存中的各道程序只能访问自己的区域，以避免相互干扰。同时要求程序能够根据需要从一个内存区移动到另一个内存区，而不影响其正确执行。

**处理器的管理和调度**。在多道程序系统中，进程的数量往往多于处理器的个数，因此进程争用处理机的情况在所难免，这就涉及到处理器的管理与分配。

**系统资源的管理和调度**。既要解决多道程序共享软硬件资源时的竞争与协作、共享与安全问题，又要解决发挥各种资源的利用率问题。



#### 简述操作系统的基本类型

**批处理操作系统**：采用批处理方式工作的操作系统

**分时操作系统**：通过把处理器的时间划分成时间片并轮流 为各个用户服务的方式工作的操作系统

**实时操作系统**：能够对外部事件或数据进行及时接受和处理，并做出反馈



### :rocket:  处理器管理

#### 进程

可并发执行的程序在某个数据集合上的一次计算活动，也是操作系统进行资源分配和保护的基本单位。

#### 进程映像

某个时刻进程的内容及其状态的集合

#### 进程控制块

操作系统用于记录和刻画进程状态及有关环境信息的数据结构，是操作系统掌握进程资料的唯一结构，也是进程存在的唯一标志。

#### 进程上下文

进程物理实体和支持进程运行的环境的总称。

#### 原语

在管态下执行、完成系统特定功能的不可被中断的过程。

#### 进程切换

处理机从一个进程的运行转到另一进程上运行，在这个过程中，进程的运行环境产生了实质变化。

#### 进程互斥

若干进程因相互争夺独占型资源而产生的竞争制约关系。

#### 进程同步

指为完成共同任务的并发进程基于某个条件来协调它们的活动，因需要在某些位置上排定执行的先后次序而等待，传递信号或消息所产生的协作制约关系。

#### 临界资源

一次仅允许一个进程使用的资源

#### 临界区

并发进程中使用临界资源的程序段

#### 进程通信

进程之间互相交换信息的工作

#### 死锁

如果一个进程集合中的每个进程都在等待只能由此集合中的其他进程才能引发的事件，而无限期陷入僵持的局面称为死锁



#### 简述进程的主要属性

**动态性**：有一定的生命周期

**共享性**：多个进程可执行同一程序，进程可以共享公共资源

**独立性**：是一个独立实体，有自己的虚存空间、程序计数器和内部状态，是资源    分配、保护和调度的基本单位

**制约性**：存在制约关系

**并发性**：执行时间上会有所重叠



#### 简述处理器调度的层次

**高级调度**：挑选进程、创建进程和作业管理的任务

**中级调度**：完成进程在内外存之间的对换工作

**低级调度**：挑选进程分配处理及等任务



#### 简述引起进程状态转换的具体原因

**运行态到等待态**：等待使用资源或某事件发生

**等待态到就绪态**：资源得到满足或某事件发生

**运行态到就绪态**：运行时间片到；出现更高优先级进程

**就绪态到运行态**：CPU空闲时选择一个就绪进程





#### 进程的基本状态有哪些？请画出进程的状态转换图

进程的基本状态有：就绪态、运行态和等待态

![yunxing](OS%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%90%8D%E8%AF%8D%E8%A7%A3%E9%87%8A&%E7%AE%80%E7%AD%94%E9%A2%98.assets/yunxing.jpg)





#### 简述临界区调度的原则

**一次至多只有一个进程进入临界区执行**

**如果已有进程在临界区中，试图进入此临界区的其他进程应等待**

**进入临界区内的进程应在有限时间内退出，以便让等待队列中的一个进程进入**



#### 简述解决死锁问题的方法

**死锁的防止**：通过限制资源申请和分配方法来使系统不会进入死锁

**死锁的避免**：对进程资源申请不加限制，但在分配之前会作安全检查，只有安全才进行分配

**死锁的检测与恢复**：对进程资源申请和分配均不加限制，但周期性地运行死锁检测程序，若发现死锁，则采用一定的策略使系统从死锁状态中解除出来。



#### 简述死锁产生的必要条件

**互斥条件**：临界资源是独占资源，进程应互斥且排他地使用这些资源。

**占有和等待条件**：进程在请求资源得不到满足而等待时，不释放已占有资源。

**不剥夺条件**：又称不可抢占，已获资源只能由进程自愿释放，不允许被其他进程剥夺。

**循环等待条件**：又称环路条件，存在循环等待链，其中每个进程都在等待链中等待下一进程所持有的资源，造成这组进程处于永远等待状态。



#### 按用途来分，信号量可分为哪些类型

**公用信号量**：联系一组并发进程，相关的进程均可在此信号量上执行P、V操作，初值通常为1，用于实现互斥

**私有信号量**：联系一组并发进程，仅允许此信号量的拥有进程执行P操作，而其它相关进程执行V操作，初值往往为0或正整数，常用于实现同步



### :rocket:  存储器管理

#### 地址重定位

程序装入时，不修改逻辑地址，只是把程序在内存中的首地址置入重定位寄存器。程序执行时，每当CPU引用内存地址时，由硬件地址转换机构将逻辑地址转换为物理地址。

#### 程序局部性

某个存储单元被访问，则该单元及其相邻单元很可能被再次访问，或最近访问过的单元很快又被再次访问。

#### 碎片

内存中不能再被使用的空闲区域称为碎片

#### 抖动

如果使用不合适的页面置换算法，会导致刚被淘汰的页面又要被调用，而调入不久又被淘汰，如此往复，使得页面的调入调出非常频繁，这种现象叫做抖动

#### 进程工作集

在某一段时间间隔内进程运行所需访问的页面集合。



#### 简述段页式存储管理技术和页式存储管理技术的不同之处

分段是信息的逻辑单位，由源程序的逻辑结构所决定，用户可见；而分页是信息的物理单位，与源程序的逻辑结构无关，用户不可见。

段长可根据用户需要来规定，段起始地址可从任何主存地址开始；而页长由系统确定，页面只能以页大小的整倍数地址开始。

分段方式中，源程序（段号，段内位移）经连结装配后地仍保持二维结构；而分页方式中，源程序（页号，页内位移）经连结装配后地址变成了一维结构。

|                     分页                     |                             分段                             |
| :------------------------------------------: | :----------------------------------------------------------: |
|                信息的物理单位                |                        信息的逻辑单位                        |
| 分页的目的是系统管理所需，为了提高内存利用率 |             分段的目的是为了更好地满足用户的需要             |
|           页的大小固定且由系统决定           | 段的长度不固定，不同的段有不同的段长，是由用户编写的程序决定的 |
|             作业地址空间是一维的             |                     作业地址空间是二维的                     |
|            有内部碎片，无外部碎片            |                    无内部碎片，有外部碎片                    |



#### 什么是Belady异常？请给出一个Belady异常的例子

使用FIFO算法进行页面置换时，增加可用物理页框数量可能会导致更多的缺页中断，这种现状叫做Belady异常。

例如对于页面的访问序列：4,3,2,1,4,3,5,4,3,2,1,5 当分配给进程的物理页框为3个时会产生9次缺页中断，而当分配给进程的物理页框为4个时会产生10次缺页中断。



#### 简述页式存储管理的基本思想

**把内存划分成相等固定大小的块/页框/页帧**

**作业被划分成和块的大小相等的页/页面**

**作业装入时，一页放入一块中，且允许作业中相邻的页放在内存中不相邻的块中**

**使用页表存放各页在内存中的首地址，以实现地址转换**



#### 简述影响缺页中断率的因素

**主存页框数/驻留集**：进程分得的页框数越多，缺页中断率越低，反之越高。

**页面大小**：页面越大，缺页中断率越低，反之则越高。

**页面替换算法**：算法的优劣直接影响缺页中断的频率大小。

**程序特性**：程序局部性好，则缺页中断率低，否则就高。



### :rocket:  设备管理

#### I/O系统

是I/O设备及其接口线路、控制部件、通道和管理软件的总称。

#### I/O操作

计算机主存和设备介质之间的数据传输操作。

#### 通道

是专门用于负责输入输出操作的一种特殊的处理机。

#### 设备独立性

即用户程序中不指定具体的物理设备，而只指定所使用的逻辑设备，由操作系统实现逻辑设备到物理设备的映射。

#### Spooling 技术

是用一类物理设备模拟另一类物理设备的技术，是将独占设备改造成共享设备的技术。



#### 简述引入缓冲技术的目的

**缓和CPU与I/O设备间速度不匹配的矛盾**

**协调逻辑记录大小与物理记录大小不一致**

**提高CPU和I/O设备的并行性**

**减少对CPU的中断频率，放宽对CPU中断响应时间的限制**。





#### I/O控制方式有哪几种？他们的主要差别是什么？

I/O控制方式包括：**轮询方式、中断方式、DMA方式和通道方式四种。**

它们的主要差别在于：**中央处理器和外围设备并行工作的方式和程度不同。**



#### 简述磁盘输入输出操作时间的构成

**寻道时间**：磁头定位磁道所需要的时间，包括启动时间和跨越磁道所需要的时间。

**旋转延迟**：当磁头寻道成功后，相应扇区到达磁头的时间。

**传输时间**：当磁头定位到相应的扇区后，完成读/写操作所需要的时间。



### :rocket:  文件管理

#### FCB

文件控制块（File Control Block，FCB）是操作系统为每个文件建立的唯一数据结构，其中包含了文件的全部文件属性。其目的是方便操作系统对文件的管理、控制和存取。

#### 目录文件

全部由目录项所构成的文件称为目录文件。目录文件不会为空，至少包含当前目录项“.”和父目录项".."

#### 文件目录

FCB汇集和组织在一起形成文件目录。

#### 文件

是由信息按一定结构组成，可持久性保存的抽象机制，由于它必定存储在某中存储设备上，故也可以认为文件是设备的一种抽象。

**文件**：文件是以计算机硬盘为载体的存储在计算机上的信息集合,，是一种抽象机制。

#### 简述Linux操作系统支持的文件类型

**普通文件**：源程序文件、数据文件、目标代码文件及操作系统文件都是普通的文件，它们通常存储在磁盘上。

**目录文件**：是由文件目录所构成的用来维护文件系统结构的系统文件。

**特殊文件**：指各种外部设备文件。可分为**块设备文件**，如存放在磁盘或光盘上的文件；**字符设备文件**，如终端、打印机等设备文件。



#### 简述文件的物理结构

文件的物理结构和组织是指逻辑文件在物理存储空间中的存放方法和组织关系。

组织方式：**组织文件、连接文件、直接文件、索引文件**



#### 什么是记录的成组与分解？记录的成组与分解带来哪些好处

若干逻辑记录合并成一组，写入一块叫做记录成组。把逻辑记录从块中分离出来的操作叫做记录分解。

好处：节省存储空间、减少I/O操作次数、提高系统效率





<hr></hr>

作者：[秋叶依剑](http://www.qiuyeyijian.com/)

授权：[署名-非商用许可证](http://creativecommons.org/licenses/by-nc/4.0/)



<hr></hr>



