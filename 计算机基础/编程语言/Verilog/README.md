## Verilog 学习

### 前言

从本质上讲，Verilog HDL 所具有的混合抽象层次由两种数据类型提供，这两种数据类型是线网（net）和变量（variable）。

对于连续性复制，变量和线网表达式能够连续地将值驱动到线网，从而提供了基本的结构化建模方法。

对于过程赋值，变量和网络值得计算结果可以存储于变量当中，从而提供了基本的行为级建模方法。

### Verilog HDL的主要功能

Verilog HDL中有两类基本数据类型：**线网数据类型和寄存器数据类型**。线网类型表示构件之间的物理连线，寄存器类型表示抽象的数据存储元件。



### Verilog HDL的数

占用的bit数就是该数的位宽；Verilog HDL 常用的基数有二进制`b或B`, 十进制 `d或D`，十六进制 `h或H`，八进制 `o或O`; 值就是该数对应的基数值

| 占用的bit数 | 分隔符 | 基数(进制) | 值   |
| ----------- | ------ | ---------- | ---- |
| 4           | '      | b          | 110  |

```verilog
4'b1010
3'd300
2'h91
```



### Verilog HDL 的系统函数

Verilog HDL的系统函数是提供给开发人员实现某些特定功能的、以` $` 开头的函数，用户在定义变量时应注意避免与系统函数名发生冲突

```verilog
$display("I love Verilog HDL!");				// 打印 I love Verilog HDL!
$finish;				//结束本次仿真
```



### Verilog HDL 数据对象

Verilog HDL 代码中使用到的数据对象有以下几种，线网型常用和默认的种类是**wire类型**

* 线网型（net）
* 寄存器型（reg）
* 存储器型
* 整型（integer）
* 时间型（time）
* 实型（real）
* 参数型（parameter）
* 字符串型（string）



### Verilog HDL 操作符



| 赋值操作符 | <=，=              |
| ---------- | ------------------ |
| 算术操作符 | +，-，*，/，%，**  |
| 逻辑操作符 | &&，\|\|，！       |
| 关系操作符 | >，<，>=，<=       |
| 相等操作符 | \==，!=，===，!\== |

**1. 赋值操作符 <=**

该操作符在进程块中使用，是非阻塞赋值。由于是在进程中使用，所以被赋值的数据对象必须是reg型。

**2. 赋值操作符 =**

该操作符在进程块或assign连续赋值语句中使用。被赋值的的数据对象可以是reg型或wire型。其中在进程块中的赋值方式是阻塞赋值。

```verilog
assign data = ~data;		//assign 语句中赋值操作符的使用

always @(data_A, data_B) begin
    data = data_A & data_B;				// 阻塞赋值
    data <= 3'b100;		//非阻塞赋值
end
```

**3.  幂运算**

使用方式 ：A ** B， 返回A的B次方的积



**4. 拼接操作符 {}**

```verilog
{s,a,b}				//拼接之后的位宽是s, a, b 三个信号的位宽之和

{2{s}}				//拼接好之后的位宽是2倍的s的位宽

{2{s}, a}			// 拼接好之后的位宽是 2*s + a
```



### Verilog HDL 的并行语句

并行语句是硬件描述语言的一大特点，他与想C语言那样的高级程序设计语言最大的不同之处是其**并行语句的执行时并列进行的，而不会因为书写顺序的先后而产生执行顺序的先后**。

Verilog HDL	 语言支持的并行语句主要有以下4种

> * assign 连续赋值语句
> * 模块实例化语句。
> * initial 语句
> * always 语句

#### assign 连续赋值语句

**只要想实现的组合电路能够用表达式写出来，使用assign连续赋值语句就能够实现其功能**

assign 连续赋值语句的三个重要应用是：

**1. 不同信号之间的连线**

**2. 组合电路的实现**

**3. 双向端口的操作**



#### initial 初始化语句

initial 初始化语句一般用于仿真测试的信号赋值，产生一些特定的信号，而不用于可综合设计。initial 语句在仿真运行中只运行一次，在仿真的0时刻开始执行，执行后立即挂起不再执行。同一文件可以有多个initial 初始化语句。



#### always 进程语句

always 语句与 initial 语句有三点不同：

* always 语句是循环执行的；
* alway 语句是可综合的
* always 语句必须有时序控制

两者相同点是：都在0时刻开始执行；都可以有一条或者多条顺序语句。



### Verilog HDL 的顺序语句

顺序语句只在仿真时间上是相对于并行语句而言的，实际硬件运行不一定顺序执行的。

Verilog HDL 语言支持的顺序语句主要有以下4种：

* 赋值语句
* if 语句
* case 语句
* 循环语句

#### 顺序赋值语句

顺序赋值语句（也称过程赋值）是出现在initial 初始化语句，always 进程及任务与函数中的赋值语句。顺序赋值语句分为阻塞顺序赋值（=）与非阻塞书序赋值语句（<=）

#### 条件语句

##### if 条件选择语句

```verilog
if (条件判断表达式)
    顺序语句 1;
else if (条件判断表达式)
    顺序语句 2;
else
    顺序语句 3;
```



##### case 条件选择语句

```verilog
always @(case_sel, a, b) begin
    case(case_sel)
        2'b00:
            dout = a & b;
        2'b01:
            dout = a | b;
        2'b10:
            dout = a ^ b;				//异或
        2'b11:
            dout = a ~^b;				//同或
        default:
            dout = a & b;
    endcase
end
```

> Verilog HDL 针对电路特性还提供了case语句的另外两种形式：`casez`和`casex`。其中`casez`语句忽略表达式两边的z部分，`casex`忽略表达式两边的x部分和z部分。即在表达式进行比较时，不将该位的状态考虑在内，这样就可以灵活地设置对信号的某些位进行比较

```verilog
casez(a)
    3'b11z: out = 1;	//当a=110,111,11z时，都有out = 1;
```

```verilog
casex(a)
    2'b1x: out=1;		//当a=10,11,1x,1z时，都有out = 1;
```



#### 循环语句

##### for 循环语句

```verilog
integer i;
integer num;
reg[7:0] data;

initial begin
    data = 8'b10010111;
end

always @(*) begin
    #10 for (i = 0; i < 8; i++) begin				//#10 代表延时10ns
        	if(data[i] = 1'b1)
            	num = num + 1;
        end
end
```



##### repeat 循环语句

repeat 循环语句直接指定将它下面的语句执行指定的次数，执行完成后退出循环

``` verilog
repeat(8) begin
    num = num + 1;
end
```



##### while 循环

```verilog
integer i;

initial begin
    i = 10;
end

#10 while(i > 0) begin				//#10 代表延时10ns
    i = i - 1;
end
```



##### forever 循环语句

forever 循环语句是永远执行的语句，没有跳出循环的机制。这个特点决定了它的一般用途是产生周期性信号，比如产生时钟信号，或者有规律的信号。

```verilog
forever
    #10 clk = ~clk;				//产生一个周期为20ns 的时钟信号
```



### Verilog HDL 代码书写规范

**模块名**

```verilog
module clk_div				//clk_div.v				//时钟分频器
module clk_8div				//clk_8div.v			//时钟8分频器
module adder				// adder.v				//加法器
module adder_8bits			//adder_8bits.v			//8位加法器
```



**端口名**

>* 输入信号用 i 作前缀，具体名放在后面，例如 i_data
>* 输入信号用 o 作前缀，具体名放在后面，例如 o_data
>* 输入信号用 io 作前缀，具体名放在后面，例如 io_data



**变量名**

线网类型以 `w_` 为前缀， 寄存器型变量以 `r_` 为前缀

| 意义     | 后缀字符串 |
| -------- | ---------- |
| 地址     | addr       |
| 数据     | data       |
| 使能     | en         |
| 计数器   | cnt        |
| 长度     | len        |
| 宽度     | wid        |
| 读       | rd         |
| 写       | wr         |
| 读有效   | ren        |
| 写有效   | wen        |
| 准备好   | rdy        |
| 接收     | rx         |
| 发送     | tx         |
| 复位     | rst        |
| 时钟     | clk        |
| 控制     | ctrl       |
| 应答     | ack        |
| 芯片选择 | ce         |
| 输出控制 | oe         |
| 写控制   | we         |



### Verilog 不可综合的语法语句

> 所谓可综合，就是Verilog HDL 综合工具能够把所综合的 Verilog HDL 代码翻译成逻辑网表

Verilog HDL 的综合工具 并不是对所有没有语法错误的代码都能够综合，Verilog HDL综合工具一般只支持部分Verilog HDL语法。在进行 Verilog HDL 可综合设计时，应避免使用以下 Verilog HDL 语法语句：

* initial 语句
* disable 语句
* fork - join 语句
* while，for，forever，repeat，循环语句
* deassign 语句
* casex， casez语句
* force, release 语句
* 'define 语句
* 代码中的时间演示控制，如“ #10 a = b”
* 变量定义是的初始化值

尽量少用 `* / %` 操作符，因为他们会占用大量资源





### 其他注意



#### 模块实例化

```verilog
flash_led_top flash_led_top(
    .clk( clk ),
    .rst_n( rst ),		//rst_n 是flash_led_top 模块的参数变量，rst是当前模块参数命令
    .sw0(sw0),
    .led( led )
);
```



#### 仿真测试

==仿真测试端口不要写到模块括号里面==

```verilog
flow_led_sim();
	reg clk;
	wire[3:0] led;
```







