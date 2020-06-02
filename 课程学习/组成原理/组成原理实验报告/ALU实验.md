###  一、实验预习报告

#### 1、实验相关知识简述

ALU是具有基本算术逻辑运算能力的电路，可以实现基本的算术运算加法、减法；基本的逻辑运算与、或、非、异或等。

#### 2、实验原理的预习情况

##### （1）实验电路设计原理及思路说明



![struct2](ALU%E5%AE%9E%E9%AA%8C.assets/struct2.png)

**ALU模块（alu_32bits.v）的控制信号**

综合考虑，确定ALU输入输出信号如下:

| 方向 |    信号名    |                             说明                             |
| ---- | :----------: | :----------------------------------------------------------: |
| 输入 |  i_a[31:0]   |                           操作数1                            |
| 输入 |  i_b[31:0]   |                           操作数2                            |
| 输入 | alu_sel[3:0] | 运算功能选择。实验任务1要求设计加/减法器电路，实验任务2要设计具有13个功能的ALU模块。所以，确定该信号位宽位4bits。 |
| 输出 |  o_y[31:0]   |                           运算结果                           |
| 输出 |      zf      |            0标志。运算结果=0时，zf=1；否则，zf=0             |
| 输出 |      cf      | 进位标志。运算结果有进位或借位时，cy=1；否则，cy=0。仅用于算术运算。 |
| 输出 |      of      | 溢出标志。运算结果溢出时，of=1；否则，of=0。仅用于算术运算。 |
| 输出 |      nf      | 负数标志。运算结果为负时，nf=1；否则，nf=0。仅用于算术运算。 |



**输入/输出缓冲器（buff.v）模块的控制信号**

综合考虑，确定输入/输出缓冲器的输入输出信号如下:

| **方向** | **信号名**   | **说明**                                             |
| -------- | ------------ | ---------------------------------------------------- |
| 输入     | i_data[31:0] | 输入数据                                             |
| 输入     | ctrl         | 输入控制，ctrl=0时，缓冲器打开；否则输出端为高阻态。 |
| 输出     | o_data       | 输出数据，三态                                       |

本次实验的输入缓冲和ALU的输出缓冲采用此模块。



**寄存器（不带输出控制）（dr.v）模块的控制信号**

综合考虑，确定寄存器（不带输出控制）的输入输出信号如下:

| **方向** | **信号名**   | **说明**                                                 |
| -------- | ------------ | -------------------------------------------------------- |
| 输入     | i_data[31:0] | 输入数据                                                 |
| 输入     | lddr         | 输入数据锁存信号。上升沿时刻，输入存入。                 |
| 输入     | rst_n        | 输入控制。rst_n=1时，寄存器复位，即输出o_data[31:0]为0。 |
| 输出     | o_data[31:0] | 输出数据                                                 |

本次实验的DR1和DR2采用此模块。

**三态缓冲寄存器（register.v）模块的控制信号**

综合考虑，确定寄存器（不带输出控制）的输入输出信号如下:

| **方向** | **信号名**   | **说明**                                                     |
| -------- | ------------ | ------------------------------------------------------------ |
| 输入     | i_data[31:0] | 输入数据                                                     |
| 输入     | ldr          | 输入数据锁存信号。上升沿时刻，输入存入。                     |
| 输入     | ctrl         | 输入控制，ctrl=0时，寄存器锁存的值输出；否则输出端为高阻态。 |
| 输出     | o_data[31:0] | 输出数据                                                     |

本次实验的R0,R1,R2和R3采用此模块。

#####  （2）实验电路原理图

根据电路功能要求设计的电路原理图如下图1所示。

![image-20200531225224723](ALU%E5%AE%9E%E9%AA%8C.assets/image-20200531225224723.png)

#### 3、实验注意事项

本次实验比较复杂，在编写Verilo代码之前需要进行仔细分析电路原理图，理清楚各个模块之间的逻辑关系。编写代码时也要注意端口之间的连接，不要出错。

### 二、实验报告

#### 1、实验目的与要求

1. 掌握算术逻辑运算单元（ALU）的工作原理。

2. 熟悉简单运算器的数据传送通路。

3. 掌握32位补码加/减法运算器的设计方法。

4. 掌握算术逻辑运算单元（ALU）电路的仿真测试方法。

#### 2、实验仪器或材料

（1）一台内存4GB以上，装有64位Windows操作系统和Vivado 2017.4以上版本软件的PC机。

（2） EG01实验板一个。

#### 3、实验原理

ALU为只有一个功能选择信号算术逻辑单元。DR1和DR2为ALU的输入暂存器。R0是通用寄存器。输入缓冲器和ALU输出缓冲器具有三态输出功能。nALU-BUS是ALU的输出缓冲信号，nSW-BUS是输入缓冲信号，nR0-BUS是寄存器R0的输出缓冲，LDR0是寄存器R0的输入锁存信号，LDDR1是寄存器DR1的输入锁存信号，LDDR2是寄存器DR2的输入锁存信号。

利用Verilog HDL描述该电路，设计代码如下：

**alu_32bits_top.v**

```verilog
module alu_32bits_top(
    ldr0, ldr1, ldr2, ldr3,
    lddr1, lddr2,
    cf, of, zf, nf,
    nr0_bus, nr1_bus, nr2_bus, nr3_bus, nalu_bus, nsw_bus,
    i_data, alu_sel, DBUS
    );

    input ldr0, ldr1, ldr2, ldr3;
    input nr0_bus, nr1_bus, nr2_bus, nr3_bus, nalu_bus, nsw_bus;
    input lddr1, lddr2;
    input [31:0] i_data;                //输入数据
    input [3:0] alu_sel;                //alu选择控制信号

    output cf, of, zf, nf;
    output wire[31:0] DBUS;

    wire [31:0] o_y;
    wire [31:0] data;
    wire [31:0] i_a;
    wire [31:0] i_b;

    alu_32bits alu_32bits(
        .i_a(i_a), .i_b(i_b), .alu_sel(alu_sel), .o_y(o_y),
        .cf(cf), .of(of), .zf(zf), .nf(nf)
    );


    register r0(
        .ldr(ldr0),
        .ctrl(nr0_bus),
        .i_data(DBUS),
        .o_data(data)
    );

    register r1(
        .ldr(ldr1),
        .ctrl(nr1_bus),
        .i_data(DBUS),
        .o_data(data)
    );

    register r2(
        .ldr(ldr2),
        .ctrl(nr2_bus),
        .i_data(DBUS),
        .o_data(data)
    );

    register r3(
        .ldr(ldr3),
        .ctrl(nr3_bus),
        .i_data(DBUS),
        .o_data(data)
    );

    dr dr1(
        .lddr(lddr1),
        .rst_n(0),
        .i_data(data),
        .o_data(i_a)
    );

    dr dr2(
        .lddr(lddr2),
        .rst_n(0),
        .i_data(data),
        .o_data(i_b)
    );

    buff input_buff(                    //输入缓冲器
        .ctrl(nsw_bus),
        .i_data(i_data),
        .o_data(DBUS)
    );
    
    buff output_buff(                   //输出缓冲器
        .ctrl(nalu_bus),
        .i_data(o_y),
        .o_data(DBUS)
    );
    
endmodule
```



**alu_32bits.v**

```verilog
module alu_32bits(
    input [31:0] i_a,                   //两个输入数据 i_a, i_b
    input [31:0] i_b,
    input [3:0] alu_sel,                //alu 功能选择信号
    output reg[31:0] o_y,               //输出结果 o_y
    output reg cf,                      //进位/借位标志, cf=1,有进位或者借位
    output reg of,                      //溢出标志, of=1, 有溢出
    output reg zf,                      //零标志, zf=1, 运算结果为0
    output reg nf                       //负数标志, nf=1, 运算结果为负数
    );

    reg[32:0] cf_temp;                  //临时变量, 用于判断进位标志cf
    reg[32:0] of_temp;                  //临时变量, 用于判断溢出标志of

    always @(i_a, i_b, alu_sel) begin
        cf <= 0;
        of <= 0;
        case (alu_sel)
            4'b0001:                    //加法
                begin
                    cf_temp <= {1'b0, i_a} + {1'b0, i_b};           //将i_a, i_b扩充一位后再相减，便于接下来判断进位/借位
                    of_temp <= {i_a[31], i_a} + {i_b[31], i_b};     //将i_a, i_b变成双符号位之后相加, 便于接下来判断溢出
                    o_y <= cf_temp[31:0];                           //相加结果
                    cf <= cf_temp[32];                              //进位/借位标志
                    of <= of_temp[32] ^ of_temp[31];                //溢出标志, 双符号位，相加后如果两个符号位不相等则溢出
                end
            4'b0010:                   //减法
                begin
                    cf_temp <= {1'b0, i_a} - {1'b0, i_b};           //将i_a, i_b扩充一位后再相减，便于接下来判断进位/借位
                    of_temp <= {i_a[31], i_a} - {i_b[31], i_b};     //将i_a, i_b变成双符号位之后相减, 便于接下来判断溢出
                    o_y <= cf_temp[31:0];                           //相减结果
                    cf <= cf_temp[32];                              //进位/借位标志
                    of <= of_temp[32] ^ of_temp[31];                //溢出标志, 双符号位，相减后如果两个符号位不相等则溢出
                end
            4'b0011:                    //加1
                begin
                    cf_temp <= {1'b0, i_a} + 1;                     //将i_a扩充一位后再加1，便于接下来判断进位/借位
                    of_temp <= {i_a[31], i_a} + 1;                  //将i_a变成双符号位之后再加1, 便于接下来判断溢出
                    o_y <= cf_temp[31:0];                           //加1结果
                    cf <= cf_temp[32];                              //进位/借位标志
                    of <= of_temp[32] ^ of_temp[31];                //溢出标志, 双符号位，相减后如果两个符号位不相等则溢出
                end
            4'b0100:                    //减1
                begin
                    cf_temp <= {1'b0, i_a} - 1;                     //将i_a扩充一位后再减1，便于接下来判断进位/借位
                    of_temp <= {i_a[31], i_a} - 1;                  //将i_a变成双符号位之后再减1, 便于接下来判断溢出
                    o_y <= cf_temp[31:0];                           //减1结果
                    cf <= cf_temp[32];                              //进位/借位标志
                    of <= of_temp[32] ^ of_temp[31];                //溢出标志, 双符号位，相减后如果两个符号位不相等则溢出
                end
            4'b0101:
                begin
                    o_y <= i_a & i_b;   //与
                end
            4'b0110:
                begin
                    o_y <= i_a | i_b;   //或
                end
            4'b0111:
                begin
                    o_y <= i_a ^ i_b;   //异或
                end
            4'b1000:
                begin
                    o_y <= ~i_a;        //取反
                end
            4'b1001:
                begin
                    o_y <= i_a << 1;    //算术左移一位
                end
            4'b1010:
                begin
                    o_y <= i_a >> 1;    //算术右移一位
                end
            4'b1011:
                begin
                    o_y <= i_a;         //直通
                end
            default:
                begin
                    o_y <= 32'bxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx;
                end
        endcase

        if(o_y == 0) begin                //零标志
            zf <= 1;
        end
        else begin
            zf <= 0;
        end

        nf <= o_y[31];                  //负数标志
    end

endmodule
```



**register.v**

```verilog
module register(
    input [31:0] i_data,                //输入数据
    input ldr,                          //输入锁存信号, 上升沿时刻, 输入存入
    input ctrl,                         //输入控制, ctrl=0时，寄存器锁存输出, 否则输出端为高阻态
    output reg[31:0] o_data             //输出数据
    );

    reg[31:0] data;                     //寄存器当中的锁存值

    always @(posedge ldr) begin         //当ldr上升沿时,就将输入的数据存入寄存器
        data <= i_data;
    end

    always @(*) begin                   //所有可能的敏感信号发生变化时, 如果控制信号ctrl为0, 就将寄存器锁存值输出; 否则为高阻态
        if(~ctrl) begin
            o_data <= data;
        end
        else begin
            o_data <= 32'bzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz;
        end
    end
endmodule
```



**dr.v**

```verilog
module dr(
    input lddr,                     //输入数据锁存信号, 上升沿时刻, 输入存入
    input rst_n,                    //复位信号, rst_n=1时, 复位信号有效
    input [31:0] i_data,            //输入数据
    output reg[31:0] o_data         //输出数据
    );

    always @(posedge lddr or posedge rst_n) begin

        // rst_n == 1? o_data[31:0] <= 32'b0 : o_data <= i_data;
        if(rst_n) begin
            o_data[31:0] <= 32'b0;
        end
        else begin
            o_data <= i_data;
        end
    end

endmodule
```



**buff.v**

```verilog

//输入/输出缓冲器
module buff(
    input ctrl,                     //控制信号, ctrl=0时, 缓冲器打开, 否则输出为高阻态
    input [31:0] i_data,            //输入数据
    output reg[31:0] o_data         //输出数据, 三态
    );

    always @(*) begin
        if(~ctrl) begin
            o_data <= i_data;
        end
        else begin
            o_data <= 32'bzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz;
        end
    end

endmodule
```



#### 4、实验过程及数据记录

本实验利用EDA工具软件Vivado完成，实验分为：电路的Verilog建模、仿真测试与分析、综合、实现、FPGA配置与下载这几个个步骤。具体为：

（1）电路的Verilog建模

依据实验任务要求，设计电路，在Vivado环境中，利用Verilog HDL建立电路模型，输入并修改预习阶段完成的Verilog HDL设计代码，保存为.v文件。

（2）仿真测试与分析

根据电路的功能，设定输入信号状态，编写测试文件，根据电路功能要求，设计如下测试代码：

**alu_32bits_top_sim.v**

```verilog

module alu_dataPath_sim( );
reg [7:0]in;
reg [3:0]alusel;
reg nsw_bus,nalu_bus,lddr1,lddr2,ldr0,ldr1,ldr2,ldr3,nr0_bus,nr1_bus,nr2_bus,nr3_bus;
wire [7:0]DBUS;
wire cy,zf,of,nf;

alu_dataPath UU(in,alusel,nsw_bus,nalu_bus,
    lddr1,lddr2,ldr0,ldr1,ldr2,ldr3,nr0_bus,nr1_bus,nr2_bus,nr3_bus,
    DBUS,cy,zf,of,nf);
    
initial begin
      alusel=1;
      #80;alusel=2;
      #80;alusel=5;
end

always begin
    in=8'h50;
    #20;in=8'h55;
    #20;in=8'hAA;
    #20;in=8'h55;
    #20;in=8'hAA;
 end
 
initial begin
    lddr1=0;
    #45;lddr1=1;#5;lddr1=0;
end
 
 initial begin
    nr0_bus=1;
    #40;nr0_bus=0;
    #20;nr0_bus=1;
 end
 
 initial begin
    lddr2=0;
    #65;lddr2=1;#5;lddr2=0;
 end
 
 initial begin
    nr1_bus=1;
    #60;nr1_bus=0;
    #20;nr1_bus=1;
 end
 
  initial begin
    nr2_bus=1;
 end
 
 initial begin
    nr3_bus=1;
 end
 
 initial begin
    ldr0=0;
    #10;ldr0=1;#10;ldr0=0;
 end
 initial begin
    ldr1=0;
    #30;ldr1=1;#10;ldr1=0;
 end
 initial begin
    ldr2=0;
 end
 initial begin
    ldr3=0;
 end
 initial begin
    nsw_bus=0;
    #40;nsw_bus=1;
 end
 
 initial begin
    nalu_bus=1;
    #50;nalu_bus=0;
    #20;nalu_bus=0;
 end
 
endmodule
```



运行仿真测试，仿真结果如图所示

![image-20200531224342507](ALU%E5%AE%9E%E9%AA%8C.assets/image-20200531224342507.png)



#### 5、实验结果分析

本次实验设计的32位算术逻辑单元（ALU）电路，可以完成基本算术运算，逻辑运算等11个功能，下面将详细分析仿真结果中完成加法运算的波形图，将涉及到的输入信号和相应的时间段制成如下表格，其他运算大致相同，就不再多说。

0~20ns，输入缓冲器的控制信号nsw_bus为低电平有效，所以在10ns时，寄存器r0的锁存信号ldr0变成高电平，将 数据i_data[31:0]=80存入到寄存器r0中。

20~40ns，同理，ldr1变成高电平，将数据i_data[31:0]=85存入到寄存器r1中。

40~60ns，nsw_bus变成高电平，数据通路DBUS[31:0]变成高阻态，表示不再接受数据的输入。寄存器r0的 输出控制信号nr0_bus变成低电平有效，在此期间，寄存器dr1的锁存信号经过了一次高电平变化，将r0的数据存到了寄存器dr1。此时还没有确定存放相加的数据的另一个寄存器dr2中的值，所以数据通路DBUS[31:0]变成未知状态。

60~80ns，同理，nr1_bus变成低电平有效，寄存器dr2的锁存信号经过了一次高电平变化，将r1的数据存到了寄存器dr2。因为ALU功能的选择信号为0001，是相加的信号，所以此后数据通路DBUS[31:0]=80+85=165。

### 三、实验总结

本实验设计了具有包含4个32位通用寄存器在内的单总线运算器数据通路。本实验所设计的跑马灯利用Vivado 2019 软件进行设计，完成了综合和仿真。通过分析仿真结果。可知该实验电路完全满足设计要求。

通过本次实验使我掌握了设计算术逻辑单元ALU的思路和基本方法，并且ALU进行基本算术逻辑运算流程在我脑海中有了清晰的认知，为今后设计简单CPU电路打下了基础。同时经过和同学交流讨论，掌握了利用Vivado设计电路时问题的分析和解决能力。

