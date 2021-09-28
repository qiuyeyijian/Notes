## 操作系统



### 1. 操作系统定义

操作系统（Operating System，OS）是指控制和管理整个计算机系统的硬件和软件资源，并合理地组织调度计算机的工作和资源分配，以提供给用户和其他软件方便的接口和环境，它是计算机系统中最基本的**系统软件**

简单来说，有三个方面

> 1. 负责管理协调硬件，软件等计算机资源的工作
> 2. 为上层应用程序、用户提供简单易用的服务
> 3. 操作系统是系统软件，而不是硬件

![image-20200315085953363](README.assets/image-20200315085953363.png)



### 2. 操作系统的特征

**并发，共享，虚拟，异步**是操作系统的四个基本特征。其中并发和共享是两个最基本的特征，二者互为存在条件。

**并发**：指两个或多个事件在同一时间间隔内发生。这些事件宏观上是同时发生的，但微观上是交替发生的。**并行**：指两个或多个时间在同一时刻同时发生。

**共享：** 即资源共享，是指系统中的资源可供内存中多个并发执行的进程共同使用。**互斥共享和同时共享**是两种资源共享方式。

所谓“同时”往往是宏观上的，而微观上，这些进程可能是交替地对该资源进行访问的（即分时共享） 

**虚拟：** 是指把一个物理上的实体变为若干个逻辑上的对应物。物理实体（前者）是实际上存在的，而逻辑上对应物（后者）是用户感受到的。

虚拟技术包括“空分复用技术”和“时分复用技术”



![image-20200315104153718](README.assets/image-20200315104153718.png)



### 3. 操作系统的发展与分类

![image-20200315105711677](README.assets/image-20200315105711677.png)



### 4. 操作系统的运行机制与体系结构

处理器的两种状态：用户态（目态，cpu只能执行非特权指令）、核心态（管态，cpu可以执行特权指令和非特权指令）。

用程序状态字寄存器（psw)中的某标志位来标识当前处理器处于什么状态，如0为用户态，1为核心态。

![image-20200315111107984](README.assets/image-20200315111107984.png)

![image-20200315111248080](README.assets/image-20200315111248080.png)



![image-20200315111733611](README.assets/image-20200315111733611.png)



### 5. 中断和异常

用户态到核心态是通过中断来实现的，并且中断是唯一的途径。

核心态到用户态的切换时通过执行一个特权指令，将程序状态字（psw）的标志位设置为“用户态”



![image-20200315174546175](README.assets/image-20200315174546175.png)



![image-20200315174639819](README.assets/image-20200315174639819.png)



![image-20200315174919762](README.assets/image-20200315174919762.png)



### 6. 系统调用



trap 指令 = 陷入指令 = 访管指令



![image-20200315175447212](README.assets/image-20200315175447212.png)



![image-20200315180727550](README.assets/image-20200315180727550.png)





![image-20200315180919269](README.assets/image-20200315180919269.png)



### 7. 进程的定义、组成和组织方式

为了方便操作系统管理、完成各程序并发执行，引入了进程、进程实体的概念。

系统为每个运行的程序配置一个数据结构，成为进程控制块（PCB），用来描述进程的各种信息（如程序代码存放位置）

PCB、程序段、数据段三部分构成了进程实体（进程映像）



![image-20200315182132015](README.assets/image-20200315182132015.png)



![image-20200315182939296](README.assets/image-20200315182939296.png)

程序段、数据段、PCB三部分组成了进程实体（进程映像）。一般情况下，我们把进程实体就简称为进程，例如，所谓创建进程，实质上是创建进程实体中的PCB; 而撤销进程，实质上是撤销进程实体中的PCB。

**PCB是进程存在的唯一标志！**

操作系统通过PCB来管理进程，因此PCB中应该包含操作系统对其进行管理所需的各种信息。



![image-20200315183838858](README.assets/image-20200315183838858.png)



![image-20200315183943200](README.assets/image-20200315183943200.png)



![image-20200315184220641](README.assets/image-20200315184220641.png)



![image-20200315184354291](README.assets/image-20200315184354291.png)

动态性是进程的基本特征，进程是资源分配，接受调度的基本单位



![image-20200315184601705](README.assets/image-20200315184601705.png)



### 8. 进程的状态

![image-20200315184923765](README.assets/image-20200315184923765.png)



![image-20200315185110723](README.assets/image-20200315185110723.png)





![image-20200315185408155](README.assets/image-20200315185408155.png)





![image-20200315185516803](README.assets/image-20200315185516803.png)



### 9. 进程控制

用原语实现进程控制。原语的特点是执行期间不允许中断，只能一气呵成。这种不可被中断的操作即原子操作。

原语采用“关中断指令”和“开中断指令”实现。关/开中断指令的权限非常强大，必然只允许在核心态下执行特权指令，所以原语运行在核心态。



![image-20200315202839518](README.assets/image-20200315202839518.png)



### 10.  进程通信



![image-20200316074624251](README.assets/image-20200316074624251.png)



![image-20200316074909579](README.assets/image-20200316074909579.png)



![image-20200316075425672](README.assets/image-20200316075425672.png)





### 11. 线程、多线程模型

线程是一个基本的cup执行单元，也是程序执行流的最小单位。引入线程之后，不仅进程之间可以并发，进程内的各个线程之间也可以并发，从而进一步提高了系统的并发度。

引入线程之后，进程只作为除cpu之外的系统资源的分配单元。也就是说，进程是资源分配的基本单位，线程是调度的基本单位。



 ![image-20200316080552950](README.assets/image-20200316080552950.png)



![image-20200316081250693](README.assets/image-20200316081250693.png)



![image-20200316081905810](README.assets/image-20200316081905810.png)





### 12. 处理机的调度



 

![image-20200316082822723](README.assets/image-20200316082822723.png)



![image-20200316083040994](README.assets/image-20200316083040994.png)



![image-20200316083145238](README.assets/image-20200316083145238.png)





### 13. 进程调度的时机

进程执行过程中若发生中断，则需要执行低级调度。



![image-20200318075245936](README.assets/image-20200318075245936.png)



![image-20200316083938973](README.assets/image-20200316083938973.png)





 抢占式和非抢占式是低级调度（进程调度）的类型，作业调度是高级调度![image-20200318080050851](README.assets/image-20200318080050851.png)



进程切换是指一个进程让出处理机，由另一个进程占用处理机的过程



![image-20200318080252571](README.assets/image-20200318080252571.png)





![image-20200318080405091](README.assets/image-20200318080405091.png)



### 14. 调度算法的评价指标



系统吞吐量：单位时间内完成作业的数量

系统吞吐量 = 总共完成了多少道作业 / 总共花了多少时间



周转时间，是指作业被提价给系统开始，到作业完成为止的这段时间间隔

![image-20200318081246140](README.assets/image-20200318081246140.png)



![image-20200318081341962](README.assets/image-20200318081341962.png)





![image-20200318081538985](README.assets/image-20200318081538985.png)





![image-20200318081648658](README.assets/image-20200318081648658.png)



### 15. 调度算法

#### 先来先服务（FCFS)

![image-20200318082523323](README.assets/image-20200318082523323.png)



 ![image-20200318082318162](README.assets/image-20200318082318162.png)



#### 短进程优先调度（SPF)

![image-20200318082823218](README.assets/image-20200318082823218.png)

#### 最短剩余时间优先（SRTN)

SRTF

![image-20200318083230620](README.assets/image-20200318083230620.png)



![image-20200318083500594](README.assets/image-20200318083500594.png)



![image-20200318083629779](README.assets/image-20200318083629779.png)





![image-20200318083804682](README.assets/image-20200318083804682.png)



#### 高响应比优先（HRRN)



![image-20200318091530351](README.assets/image-20200318091530351.png)



![image-20200318084130545](README.assets/image-20200318084130545.png)



![image-20200318084418343](README.assets/image-20200318084418343.png)

#### 时间片轮转（RR)



![image-20200320113221961](README.assets/image-20200320113221961.png)



![image-20200320112956873](README.assets/image-20200320112956873.png)

一般来说，设计时间片时要让切换进程的开销占比不超过1%



#### 优先级调度



![image-20200320114321793](README.assets/image-20200320114321793.png)



![image-20200320113618503](README.assets/image-20200320113618503.png)





![image-20200320114229529](README.assets/image-20200320114229529.png)





### 多级反馈队列

![image-20200320115625073](README.assets/image-20200320115625073.png)



![image-20200320120505518](README.assets/image-20200320120505518.png)



### 作业，进程，线程，管程

 作业：用户在一次解决或是一个事务处理过程中要求计算机系统所做的工作的集合，它包括用户程序、所需要的数据集控制命令等。作业是由一系列有序的步骤组成的。在执行一个作业可能会运行多个不同的进程。

​     进程：程序在一个数据集上的一次运行过程。是操作系统资源分配的基本单位。

​     线程：是进程中的一个实体，是被操作系统独立调度和执行的基本单位。一个进程包含一个或多个线程。

​      线程特征：

1、线程的执行状态包括运行、就绪和等待。

2、进程中的所有线程共享所属进程内的主存和其他资源。

3、拥有自己的线程控制块和执行栈，寄存器。

​      进程和线程的区别：

1、进程间是独立的，在内存空间、上下文环境上。而线程是运行在进程空间内的，同一进程所产生的线程共享同一内存空间。

2、进程间是可以并发执行的，线程之间也可以并发执行。但同一进程中的两端代码只有在引入线程的情况下才能并发执行。

3、线程是属于进程的，当进程退出时，该进程所产生的线程都会被强制退出并清除。

4、线程占用的资源要少于进程占用的资源，线程间的切换速度比进程间的切换快的多。

​    管程：是定义了一个数据结构和在该数据结构上能为并发进程所执行的一组操作。这些操作能同步进程和改变管程中的数据。它是一种进程同步机制。在结构上类似于面向对象中的类。在功能上和信号量和p，v操作类似。可以更方便的管理系统的临界资源。

​     管程特征：

1、模块化：一个管程就是一个可单独编译的实体，结构上和类相仿。

2、抽象数据类型。

3、信息隐蔽。

​    管程要素：

1、安全性：管程中的数据变量在管程之外是不可见的，只能有该管程的操作过程存取。

2、互斥性：任一时刻只能有一个调用者进入管程。

3、等待机制：设置等待队列及相应的操作，对资源进行管理。

​    管程和上面三个名词，区别大，但是容易混淆。

​    管道：是一种进程通信机制。是共享文件模式，它基于文件系统，在两个进程之间，以先进先出的方式实现消息的单向传送。管道是一种特殊文件。

### 进程同步

![image-20200323070030900](README.assets/image-20200323070030900.png)



### 进程互斥

![image-20200323065951748](README.assets/image-20200323065951748.png)

![image-20200323065914080](README.assets/image-20200323065914080.png)

临界区是进程中访问临界资源的代码段

进入区和退出区是负责实现互斥的代码段



![image-20200323072223564](README.assets/image-20200323072223564.png)

![image-20200323072324843](README.assets/image-20200323072324843.png)



![image-20200325080233181](README.assets/image-20200325080233181.png)



### 进程互斥的软件实现



![image-20200323073224837](README.assets/image-20200323073224837.png)

单标志法可以实现 同一时刻最多只允许一个进程访问临界区。



![image-20200325084057931](README.assets/image-20200325084057931.png)



![image-20200325090638547](README.assets/image-20200325090638547.png)



![image-20200325091444646](README.assets/image-20200325091444646.png)



![image-20200325091539482](README.assets/image-20200325091539482.png)



### 进程互斥的硬件实现方法



![image-20200325091928964](README.assets/image-20200325091928964.png)



![image-20200325092418548](README.assets/image-20200325092418548.png)



![image-20200325092630260](README.assets/image-20200325092630260.png)



![image-20200325092655534](README.assets/image-20200325092655534.png)



### 信号量机制



![image-20200405205200200](README.assets/image-20200405205200200.png)





![image-20200405205904156](README.assets/image-20200405205904156.png)



![image-20200405210240205](README.assets/image-20200405210240205.png)



![image-20200405210954744](README.assets/image-20200405210954744.png)



![image-20200405211138622](README.assets/image-20200405211138622.png)





### 用信号量机制实现进程互斥、进程同步、进程同步关系



![image-20200406084608161](README.assets/image-20200406084608161.png)



![image-20200406085125323](README.assets/image-20200406085125323.png)



![image-20200406085601180](README.assets/image-20200406085601180.png)



![image-20200406085758348](README.assets/image-20200406085758348.png)



### 生产者 消费者问题



![image-20200406090755330](README.assets/image-20200406090755330.png)



![image-20200406091248929](README.assets/image-20200406091248929.png)



![image-20200406091654247](README.assets/image-20200406091654247.png)



![image-20200406092316663](README.assets/image-20200406092316663.png)



实现互斥的P操作一定要在实现同步的P操作之后



![image-20200406092814748](README.assets/image-20200406092814748.png)





### 多生产者 多消费者问题



![image-20200406093148303](README.assets/image-20200406093148303.png)



![image-20200406093901055](README.assets/image-20200406093901055.png)



**如果缓冲区大于1， 就必须专门设置一个互斥信号量 mutex 来保证互斥访问缓冲区**



![image-20200406094500484](README.assets/image-20200406094500484.png)





### 吸烟者问题



![image-20200406094934903](README.assets/image-20200406094934903.png)





### 读者 写者问题



![image-20200406095809975](README.assets/image-20200406095809975.png)



### 管程



![image-20200426162526145](README.assets/image-20200426162526145.png)



![image-20200426163231931](README.assets/image-20200426163231931.png)





![image-20200426163352905](README.assets/image-20200426163352905.png)



![image-20200426163422833](README.assets/image-20200426163422833.png)





### 死锁的概念



![image-20200426163912267](README.assets/image-20200426163912267.png)



![image-20200426164143475](README.assets/image-20200426164143475.png)



![image-20200426164452565](README.assets/image-20200426164452565.png)



![image-20200426164758220](README.assets/image-20200426164758220.png) 

![image-20200426165047676](README.assets/image-20200426165047676.png)



### 预防死锁



![image-20200426165125089](README.assets/image-20200426165125089.png)



![image-20200426165337429](README.assets/image-20200426165337429.png)



![image-20200426165544293](README.assets/image-20200426165544293.png)



![image-20200426180423357](README.assets/image-20200426180423357.png)

![image-20200426180751892](README.assets/image-20200426180751892.png)

![image-20200426180835539](README.assets/image-20200426180835539.png)



### 避免死锁



![image-20200426180925498](README.assets/image-20200426180925498.png)



![image-20200426181432028](README.assets/image-20200426181432028.png)

![image-20200426181851708](README.assets/image-20200426181851708.png)



![image-20200426182112223](README.assets/image-20200426182112223.png)



![image-20200426182306219](README.assets/image-20200426182306219.png)



### 死锁的检测和解除



![image-20200426182402925](README.assets/image-20200426182402925.png)

![image-20200426182852445](README.assets/image-20200426182852445.png)

![image-20200426183005989](README.assets/image-20200426183005989.png)

![image-20200426183239159](README.assets/image-20200426183239159.png)

![image-20200426183318696](README.assets/image-20200426183318696.png)







### 内存的基础知识



![image-20200408203309107](README.assets/image-20200408203309107.png)



![image-20200408204658600](README.assets/image-20200408204658600.png)



![image-20200408204238654](README.assets/image-20200408204238654.png)



![image-20200408215608599](README.assets/image-20200408215608599.png)



![image-20200408204604521](README.assets/image-20200408204604521.png)



![image-20200408204831303](README.assets/image-20200408204831303.png)



![image-20200408205238639](README.assets/image-20200408205238639.png)



### 内存管理



![image-20200408205611855](README.assets/image-20200408205611855.png)



![image-20200408205949876](README.assets/image-20200408205949876.png)



![image-20200408210204125](README.assets/image-20200408210204125.png)



![image-20200408210320288](README.assets/image-20200408210320288.png)



### 覆盖和交换



![image-20200408210521117](README.assets/image-20200408210521117.png)



![image-20200408210951157](README.assets/image-20200408210951157.png)



![image-20200408211305265](README.assets/image-20200408211305265.png)



![image-20200408211415036](README.assets/image-20200408211415036.png)



### 



![image-20200408211602568](README.assets/image-20200408211602568.png)

### 内存管理，连续分配

![image-20200408211953550](README.assets/image-20200408211953550.png)



![image-20200408212350590](README.assets/image-20200408212350590.png)



![image-20200408212629773](README.assets/image-20200408212629773.png)



![image-20200408212912898](README.assets/image-20200408212912898.png)



![image-20200408213615879](README.assets/image-20200408213615879.png)



![image-20200408213730607](README.assets/image-20200408213730607.png)



### 动态分区分配算法



![image-20200410195817810](README.assets/image-20200410195817810.png)



![image-20200410200405266](README.assets/image-20200410200405266.png)



![image-20200410200901145](README.assets/image-20200410200901145.png)



![image-20200410201005462](README.assets/image-20200410201005462.png)



![image-20200410201324681](README.assets/image-20200410201324681.png)



![image-20200410201423057](README.assets/image-20200410201423057.png)



### 基本分页存储管理



![image-20200410201627467](README.assets/image-20200410201627467.png)



![image-20200410201821853](README.assets/image-20200410201821853.png)



![image-20200410202134676](README.assets/image-20200410202134676.png)



![image-20200410211301491](README.assets/image-20200410211301491.png)



![image-20200410211640774](README.assets/image-20200410211640774.png)



![image-20200410211822091](README.assets/image-20200410211822091.png)



![image-20200410211958743](README.assets/image-20200410211958743.png)



![image-20200410212137257](README.assets/image-20200410212137257.png)



![image-20200410212322973](README.assets/image-20200410212322973.png)



### 基本地址变换机构



![image-20200410212424954](README.assets/image-20200410212424954.png)



![image-20200410213008581](README.assets/image-20200410213008581.png)



![image-20200410213131809](README.assets/image-20200410213131809.png)



![image-20200410213408912](README.assets/image-20200410213408912.png)



![image-20200410213636501](README.assets/image-20200410213636501.png)



![image-20200410213910075](README.assets/image-20200410213910075.png)



![image-20200410214059806](README.assets/image-20200410214059806.png)



### 具有快表的地址变换机构

![image-20200410214429878](README.assets/image-20200410214429878.png)



快表，又称联想寄存器（TLB)， 是一种访问速度比内存快很多的高速缓冲器，用来存放当前访问的若干页表项，以加速地址变化的过程。与此对应，内存中的页表常称为慢表。



![image-20200411091018581](README.assets/image-20200411091018581.png)



![image-20200411091349695](README.assets/image-20200411091349695.png)



![image-20200411091449742](README.assets/image-20200411091449742.png)



### 两级页表



![image-20200411091717462](README.assets/image-20200411091717462.png)



![image-20200411092018453](README.assets/image-20200411092018453.png)



![image-20200411092316960](README.assets/image-20200411092316960.png)



![image-20200411093232727](README.assets/image-20200411093232727.png)



![image-20200411093458411](README.assets/image-20200411093458411.png)



![image-20200411093805845](README.assets/image-20200411093805845.png)



![image-20200411093840199](README.assets/image-20200411093840199.png)



### 基本分段存储管理方式



![image-20200411094123267](README.assets/image-20200411094123267.png)



![image-20200411094746877](README.assets/image-20200411094746877.png)



![image-20200411095556412](README.assets/image-20200411095556412.png)



![image-20200411100017660](README.assets/image-20200411100017660.png)



![image-20200411100309724](README.assets/image-20200411100309724.png)



![image-20200411100741386](README.assets/image-20200411100741386.png)



![image-20200411100856185](README.assets/image-20200411100856185.png)



![image-20200411101004110](README.assets/image-20200411101004110.png)



![image-20200411101107690](README.assets/image-20200411101107690.png)



### 段页式管理方式

![image-20200411101534636](README.assets/image-20200411101534636.png)



![image-20200411101733514](README.assets/image-20200411101733514.png)



![image-20200411102615331](README.assets/image-20200411102615331.png)



![image-20200411102754960](README.assets/image-20200411102754960.png)



### 虚拟内存的基本概念



![image-20200411143850713](README.assets/image-20200411143850713.png)



![image-20200411144232771](README.assets/image-20200411144232771.png)



![image-20200411144449065](README.assets/image-20200411144449065.png)



![image-20200411144649352](README.assets/image-20200411144649352.png)



![image-20200411144803551](README.assets/image-20200411144803551.png)



### 请求分页管理方式

![image-20200411144917789](README.assets/image-20200411144917789.png)



![image-20200411145130911](README.assets/image-20200411145130911.png)



![image-20200411145530329](README.assets/image-20200411145530329.png)



![image-20200411145737361](README.assets/image-20200411145737361.png)



![image-20200411145929367](README.assets/image-20200411145929367.png)



![image-20200411150315576](README.assets/image-20200411150315576.png)



![image-20200411150436003](README.assets/image-20200411150436003.png)



### 页面置换算法

![image-20200411150604021](README.assets/image-20200411150604021.png)



![image-20200411165334287](README.assets/image-20200411165334287.png)

最佳置换算法可以保证最低的缺页率，但实际上，只有在进程执行的过程中才能知道接下来会访问到的那个页面。操作系统无法提前预判页面访问序列。因此，最佳置换算法是无法实现的。



![image-20200411165848809](README.assets/image-20200411165848809.png)



![image-20200411171137919](README.assets/image-20200411171137919.png)



![image-20200411171543870](README.assets/image-20200411171543870.png)



![image-20200411172048405](README.assets/image-20200411172048405.png)



![image-20200411172124862](README.assets/image-20200411172124862.png)



![image-20200411172205300](README.assets/image-20200411172205300.png)



### 页面分配策略

![image-20200411172538105](README.assets/image-20200411172538105.png)



![image-20200411172853823](README.assets/image-20200411172853823.png)



![image-20200411173157196](README.assets/image-20200411173157196.png)



![image-20200411173334768](README.assets/image-20200411173334768.png)



![image-20200411173524989](README.assets/image-20200411173524989.png)



![image-20200411173617279](README.assets/image-20200411173617279.png)



![image-20200411173802701](README.assets/image-20200411173802701.png)



![image-20200411173949624](README.assets/image-20200411173949624.png)



### 文件管理

![image-20200411174221394](README.assets/image-20200411174221394.png)



![image-20200411174755702](README.assets/image-20200411174755702.png)

![image-20200411175220934](README.assets/image-20200411175220934.png)

![image-20200411175558487](README.assets/image-20200411175558487.png)



### 文件的逻辑结构

![image-20200411180528442](README.assets/image-20200411180528442.png)



![image-20200411180824308](README.assets/image-20200411180824308.png)



![image-20200411202142244](README.assets/image-20200411202142244.png)



### 文件目录

树形目录结构可以很方便地对文件进行分类，层次结构清晰，也能够更有效地进行文件的管理和保护。但是，树形结构不方便于文件的共享。为此，提出了“无环图目录结构”

![image-20200411204331883](README.assets/image-20200411204331883.png)



### 文件的物理结构



![image-20200411204551324](README.assets/image-20200411204551324.png)

![image-20200411204711664](README.assets/image-20200411204711664.png)



![image-20200411205106213](README.assets/image-20200411205106213.png)



![image-20200411205449458](README.assets/image-20200411205449458.png)



![image-20200411205956519](README.assets/image-20200411205956519.png)



![image-20200412091740398](README.assets/image-20200412091740398.png)



![image-20200412092228748](README.assets/image-20200412092228748.png)



![image-20200412092829798](README.assets/image-20200412092829798.png)



![image-20200412093505910](README.assets/image-20200412093505910.png)



![image-20200412093904155](README.assets/image-20200412093904155.png)



![image-20200412094127450](README.assets/image-20200412094127450.png)



![image-20200412094240882](README.assets/image-20200412094240882.png)



### 文件存储空间管理

![image-20200413074516478](README.assets/image-20200413074516478.png)



![image-20200413075525169](README.assets/image-20200413075525169.png)



![image-20200413080407040](README.assets/image-20200413080407040.png)



![image-20200413080507294](README.assets/image-20200413080507294.png)



![image-20200413081209541](README.assets/image-20200413081209541.png)



### 文件的基本操作

![image-20200413081648223](README.assets/image-20200413081648223.png)



![image-20200413081854217](README.assets/image-20200413081854217.png)



![image-20200413084400516](README.assets/image-20200413084400516.png)



![image-20200413084455427](README.assets/image-20200413084455427.png)

![image-20200413084713694](README.assets/image-20200413084713694.png)



![image-20200413084903932](README.assets/image-20200413084903932.png)





### 文件共享

![image-20200413090650822](README.assets/image-20200413090650822.png)



### 文件保护

![image-20200413091742648](README.assets/image-20200413091742648.png)







### 磁盘的结构

![image-20200414214717618](README.assets/image-20200414214717618.png)



![image-20200414215451425](README.assets/image-20200414215451425.png)



![image-20200414215613647](README.assets/image-20200414215613647.png)



![image-20200414215823006](README.assets/image-20200414215823006.png)



### 磁盘调度算法

![image-20200414220016633](README.assets/image-20200414220016633.png)



![image-20200414220637524](README.assets/image-20200414220637524.png)



![image-20200414221034075](README.assets/image-20200414221034075.png)



![image-20200414221338094](README.assets/image-20200414221338094.png)



![image-20200414221704703](README.assets/image-20200414221704703.png)



![image-20200414221852893](README.assets/image-20200414221852893.png)



![image-20200414222448614](README.assets/image-20200414222448614.png)



### 减少磁盘延迟时间



 ![image-20200414223400833](README.assets/image-20200414223400833.png)



![image-20200414223854662](README.assets/image-20200414223854662.png)





![image-20200414223959114](README.assets/image-20200414223959114.png)



![image-20200414224021444](README.assets/image-20200414224021444.png)



![image-20200414224341043](README.assets/image-20200414224341043.png)



![image-20200414224416717](README.assets/image-20200414224416717.png)



### 磁盘的管理

![image-20200414224631629](README.assets/image-20200414224631629.png)



![image-20200414224901971](README.assets/image-20200414224901971.png)



![image-20200414225038694](README.assets/image-20200414225038694.png)



![image-20200414225110161](README.assets/image-20200414225110161.png)



### I-O设备的概念和分类

![image-20200415080737149](README.assets/image-20200415080737149.png)



![image-20200415080903202](README.assets/image-20200415080903202.png)



![image-20200415080950018](README.assets/image-20200415080950018.png)



![image-20200415081039445](README.assets/image-20200415081039445.png)



![image-20200415081231075](README.assets/image-20200415081231075.png)



### I-O控制器

![image-20200415082610912](README.assets/image-20200415082610912.png)



![image-20200418181600356](README.assets/image-20200418181600356.png)

 

![image-20200418181652918](README.assets/image-20200418181652918.png)



![image-20200418181833011](README.assets/image-20200418181833011.png)



![image-20200418181917698](README.assets/image-20200418181917698.png)



### I-O控制方式 



![image-20200418184531852](README.assets/image-20200418184531852.png)



![image-20200418185216945](README.assets/image-20200418185216945.png)



![image-20200418185427813](README.assets/image-20200418185427813.png)



![image-20200418185853951](README.assets/image-20200418185853951.png)



![image-20200418190130914](README.assets/image-20200418190130914.png)



![image-20200418190313231](README.assets/image-20200418190313231.png)



![image-20200418190507729](README.assets/image-20200418190507729.png)



![image-20200418190622014](README.assets/image-20200418190622014.png)



![image-20200418190801350](README.assets/image-20200418190801350.png)



### I_O软件层次

  

![image-20200418191105907](README.assets/image-20200418191105907.png)



![image-20200418191714541](README.assets/image-20200418191714541.png)



![image-20200418191836578](README.assets/image-20200418191836578.png)



![image-20200418192001556](README.assets/image-20200418192001556.png)



### I_O核心子系统



![image-20200418192059014](README.assets/image-20200418192059014.png)



![image-20200418192227168](README.assets/image-20200418192227168.png)



![image-20200418192321354](README.assets/image-20200418192321354.png)



### 假脱机技术



![image-20200418192626414](README.assets/image-20200418192626414.png)



![image-20200418192936604](README.assets/image-20200418192936604.png)



![image-20200418193310177](README.assets/image-20200418193310177.png)



![image-20200418193416547](README.assets/image-20200418193416547.png)



### 设备的分配与回收



![image-20200418193551633](README.assets/image-20200418193551633.png)



![image-20200418193803819](README.assets/image-20200418193803819.png)



![image-20200418193920973](README.assets/image-20200418193920973.png)



![image-20200418194422536](README.assets/image-20200418194422536.png)



![image-20200418194458884](README.assets/image-20200418194458884.png)



![image-20200418194542602](README.assets/image-20200418194542602.png)



![image-20200418194248914](README.assets/image-20200418194248914.png)



![image-20200418194748394](README.assets/image-20200418194748394.png)



![image-20200418194855630](README.assets/image-20200418194855630.png)



![image-20200418194956244](README.assets/image-20200418194956244.png)



![image-20200418195020130](README.assets/image-20200418195020130.png)



![image-20200418195227266](README.assets/image-20200418195227266.png)



### 缓冲区管理



![image-20200418195339778](README.assets/image-20200418195339778.png)



![image-20200418195521230](README.assets/image-20200418195521230.png)



![image-20200418195736656](README.assets/image-20200418195736656.png)



![image-20200418200019899](README.assets/image-20200418200019899.png)



![image-20200418200409657](README.assets/image-20200418200409657.png)



![image-20200418200400435](README.assets/image-20200418200400435.png)



![image-20200418200624948](README.assets/image-20200418200624948.png)



![image-20200418200832053](README.assets/image-20200418200832053.png)



![image-20200418200851534](README.assets/image-20200418200851534.png)





