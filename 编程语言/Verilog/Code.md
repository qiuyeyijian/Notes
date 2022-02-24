## Verilog HDL 代码和仿真文件





### 偶分频器

#### 设计文件

```verilog
module divider(
    input wire clk,
    input wire rst_n,
    output reg clk_div
    );

    parameter NUM_DIV = 4;		//4分频
    integer cnt;                // 计数

    always @(posedge clk or posedge rst_n) begin
        if(rst_n) begin				//复位信号，高电平有效
            cnt = 0;
            clk_div <= 1'b0;
        end
        else if(cnt < NUM_DIV / 2 -1) begin
            cnt = cnt + 1;
            clk_div <= clk_div;
        end
        else begin
            cnt = 0;
            clk_div <= ~clk_div;
        end
    end
endmodule

```

#### 仿真文件

```verilog
module divider_sim(
);
    reg clk;
    reg rst_n;
    wire clk_div;

    divider divider(
        .clk(clk),
        .rst_n(rst_n),
        .clk_div(clk_div)
     );

     initial begin
         clk <= 1;
         rst_n <= 0;
         #1 rst_n <= 1;     //快速进行复位操作，防止分频器clk_div 一直表现为高阻态
         #1 rst_n <= 0;
     end

     always #10 clk <= ~clk;		//设置时钟周期为20ns,即主频为50MHz
endmodule

```





### 3-8译码器

#### 设计文件

```verilog
module decode38a(
    input wire[2:0] a,
    output reg[7:0] y
    );
    /*
    第一种方法，按照公式，进行数据流描述，代码较多，但是可综合

    assign y[0] = ~a[2] & ~a[1] & ~a[0];
    assign y[1] = ~a[2] & ~a[1] &  a[0];    
    assign y[2] = ~a[2] &  a[1] & ~a[0];
    assign y[3] = ~a[2] &  a[1] &  a[0];
    assign y[4] =  a[2] & ~a[1] & ~a[0];
    assign y[5] =  a[2] & ~a[1] &  a[0];    
    assign y[6] =  a[2] &  a[1] & ~a[0];
    assign y[7] =  a[2] &  a[1] &  a[0];         
    */

    /*第二种，用 for循环, 方法简单，所用代码较少，但是不可综合 */

    integer i;
    always@(*) begin
        for(i = 0; i <= 7; i = i + 1) begin
            if (a == i)
                y[i] = 1;
            else
                y[i] = 0;
        end
    end
endmodule
```

#### 仿真文件

```verilog
module decode38_simulation(
    );
    reg[2:0] a;
    wire[7:0] y;

    decode38a decode38a(
        .a(a),
        .y(y)
    );
    initial
    begin
        #0      a <= 3'b000;
        #100    a <= 3'b001;
        #100    a <= 3'b010;
        #100    a <= 3'b011;
        #100    a <= 3'b100;
        #100    a <= 3'b101;
        #100    a <= 3'b110;
        #100    a <= 3'b111;                                                          
    end
endmodule
```





### 流水灯

一共有四个LED灯：

* 首先1、3 灯亮，然后2、4 灯亮，接着1、2、3、4灯依次点亮，然后全灭，最后再点亮
* 重复上一步

因为时钟周期20ns，基本上肉眼看不到效果，所以设置了一个4分频器，用户可以修改变量`NUM_DIV`的值，实现自己想要的分频效果

#### 设计文件

**顶层模块 flow_led_top.v**

```verilog
module flow_led_top(
    input clk,
    input rst_n,
    output [3:0] led
    );
    
    wire clk_div;
    
    //调用分频器模块
    divider divider(
        .clk(clk),
        .rst_n(rst_n),
        .clk_div(clk_div)
    );
    
    //调用流水灯的控制模块
    flow_led flow_led(
        .clk(clk),
        .clk_div(clk_div),
        .led(led)
    );

endmodule
```



**分频器模块 divider.v**

```verilog
module divider(
    input wire clk,
    input wire rst_n,
    output reg clk_div
    );

    parameter NUM_DIV = 4;		//4分频
    integer cnt;                // 计数

    always @(posedge clk or posedge rst_n) begin
        if(rst_n) begin				//复位信号，高电平有效
            cnt = 0;
            clk_div <= 1'b0;
        end
        else if(cnt < NUM_DIV / 2 -1) begin
            cnt = cnt + 1;
            clk_div <= clk_div;
        end
        else begin
            cnt = 0;
            clk_div <= ~clk_div;
        end
    end
endmodule

```



**流水灯控制模块 flow_led.v**

```verilog
module flow_led(
    input clk,
    input rst_n,
    output reg[3:0] led
    );

    //定义当前状态，下一个状态
    reg[10:0] current_state, next_state;

    //12种状态编码
    parameter s0 = 11'b00000000000,
            s1 = 11'b00000000001,
            s2 = 11'b00000000010,
            s3 = 11'b00000000100,
            s4 = 11'b00000001000,
            s5 = 11'b00000010000,
            s6 = 11'b00000100000,
            s7 = 11'b00001000000,
            s8 = 11'b00010000000,
            s9 = 11'b00100000000,
            s10 = 11'b01000000000,
            s11 = 11'b10000000000;

    always @(posedge clk or posedge rst_n) begin
        if(rst_n) begin				//复位信号，高电平有效
            current_state <= s0;
        end else begin
            current_state <= next_state;
        end
    end
    
    always @(current_state) begin
        case(current_state)
            s0: next_state <= s1;
            s1: next_state <= s2;
            s2: next_state <= s3;
            s3: next_state <= s4;
            s4: next_state <= s5;
            s5: next_state <= s6;
            s6: next_state <= s7;
            s7: next_state <= s8;
            s8: next_state <= s9;
            s9: next_state <= s10;
            s10: next_state <= s11;
            s11: next_state <= s0;
            default: next_state <=s0;
        endcase
    end

    always @(posedge clk) begin
        case(current_state)
            s0: led <= 4'b0101;
            s1: led <= 4'b1010;
            s2: led <= 4'b0001;
            s3: led <= 4'b0011;
            s4: led <= 4'b0111;
            s5: led <= 4'b1111;
            s6: led <= 4'b1110;
            s7: led <= 4'b1100;
            s8: led <= 4'b1000;
            s9: led <= 4'b0000;
            s10: led <= 4'b1111;
            s11: led <= 4'b0000;
            default: led <= 4'b0000;
        endcase
    end

endmodule
```



#### 仿真文件

**flow_led_sim.v**

```verilog
module flow_led_sim(
    );
    reg clk;
    wire[3:0] led;
    reg rst_n;
    wire clk_div;

    divider divider(
        .clk(clk),
        .rst_n(rst_n),
        .clk_div(clk_div)
    );

    flow_led_top flow_led_top(
        .clk(clk),
        .rst_n(rst_n),
        .led(led)
    );

    initial begin
        clk <= 1;
        rst_n <= 0;
        //开始的时候电路有的信号为高阻态, 如果不复位的话可能会导致仿真不成功
        //下面的代码相当于快速按下复位按钮
        #1 rst_n <= 1;
        #1 rst_n <= 0;
    end

    //设置时钟周期 100ns
    always #50 clk <= ~clk;


endmodule
```





### ALU 算术逻辑单元

#### 设计文件

**顶层模块 alu_32bits_top.v**

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



**ALU控制模块 alu_32bits.v**

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



**通用寄存器模块 register.v**

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



**数据寄存器模块 dr.v**

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



**输入输出缓冲器模块 buff.v**

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



#### 仿真文件

**alu_32bits_top_sim.v**

```verilog
module alu_32bits_top_sim(
    );

    reg [31:0] i_data;
    reg [3:0] alu_sel;
    reg ldr0, ldr1, ldr2, ldr3, lddr1, lddr2;
    reg nr0_bus, nr1_bus, nr2_bus, nr3_bus, nalu_bus, nsw_bus;

    wire [31:0]DBUS;
    wire cf,zf,of,nf;

    alu_32bits_top alu_32bits_top(
        .ldr0(ldr0), .ldr1(ldr1), .ldr2(ldr2), .ldr3(ldr3),
        .lddr1(lddr1), .lddr2(lddr2),
        .cf(cf), .of(of), .zf(zf), .nf(nf),
        .nr0_bus(nr0_bus), .nr1_bus(nr1_bus), .nr2_bus(nr2_bus), .nr3_bus(nr3_bus), .nalu_bus(nalu_bus), .nsw_bus(nsw_bus),
        .i_data(i_data), .alu_sel(alu_sel), .DBUS(DBUS)
    );

        
    initial begin
        #0  alu_sel=1;
        #80;alu_sel=2;
        #80;alu_sel=5;
    end

    always begin
        #0  i_data=8'h50;
        #20;i_data=8'h55;
        #20;i_data=8'hAA;
        #20;i_data=8'h55;
        #20;i_data=8'hAA;
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
        #0  nalu_bus=1;
        #50;nalu_bus=0;
        #20;nalu_bus=0;
    end
    
endmodule
```

