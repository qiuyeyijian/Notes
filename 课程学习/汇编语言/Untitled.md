4.1 指出下列指令的错误：

(1) MOV AH, BX ；寄存器类型不匹配

(2) MOV [BX], [SI] ；不能都是存储器操作数

(3) MOV AX, [SI][DI] ；[SI]和[DI]不能一起使用

(4) MOV MYDAT [BX][SI], ES:AX ；AX寄存器不能使用段超越

(5) MOV BYTE PTR [BX], 1000 ；1000超过了一个字节的范围

(6) MOV BX, OFFSET MYDAT [SI] ；MYDAT [SI]已经是偏移地址,不能再使用OFFSET

(7) MOV CS, AX ；CS不能用作目的寄存器

(8) MOV ECX, AX ；两个操作数的数据类型不同

答：见注释。

4.2 下面哪些指令是非法的？(假设OP1，OP2是已经用DB定义的变量)

(1) CMP 15, BX ；错，立即数不能作为目的操作数

(2) CMP OP1, 25

(3) CMP OP1, OP2 ；错，不能都是存储器操作数

(4) CMP AX, OP1 ；错，类型不匹配，应为CMP ax, word ptr op1

答：见注释。

4.3 假设下列指令中的所有标识符均为类型属性为字的变量，请指出下列哪些指令是非法的？它们的错误是什么？

(1) MOV BP, AL ；错，寄存器类型不匹配

(2) MOV WORD_OP [BX+4*3][DI], SP

(3) MOV WORD_OP1, WORD_OP2 ；错，不能都是存储器操作数

(4) MOV AX, WORD_OP1[DX] ；错，DX不能用于存储器寻址

(5) MOV SAVE_WORD, DS

(6) MOV SP, SS:DATA_WORD [BX][SI]

(7) MOV [BX][SI], 2 ；错，[BX][SI]未指出数据类型

(8) MOV AX, WORD_OP1+WORD_OP2

(9) MOV AX, WORD_OP1-WORD_OP2+100

(10) MOV WORD_OP1, WORD_OP1-WORD_OP2

答：见注释。

4.4 假设VAR1和VAR2为字变量，LAB为标号，试指出下列指令的错误之处：

(1) ADD VAR1, VAR2 ；不能都是存储器操作数

(2) SUB AL, VAR1 ；数据类型不匹配

(3) JMP LAB [SI] ；LAB是标号而不是变量名，后面不能加[SI]

(4) JNZ VAR1 ；VAR1是变量而不是标号

(5) JMP NEAR LAB ；应使用NEAR PTR

答：见注释。

4.5 画图说明下列语句所分配的存储空间及初始化的数据值。

(1) BYTE_VAR DB ‘BYTE’,12,-12H,3 DUP(0,?,2 DUP(1,2),?)

(2) WORD_VAR DW 5 DUP(0,1,2),?,-5,‘BY’,‘TE’,256H

答：答案如下图所示。

4.6 试列出各种方法，使汇编程序把5150H存入一个存储器字中(如：DW 5150H)。

4.5题答案

42H

59H

54H

45H

0DH

EEH

00H

\-

01H

02H

01H

02H

\-

00H

\-

01H

02H

01H

02H

\-



BYTE_VAR

00H

00H

01H

00H

02H

00H

┇

┇

┇

\-

\-

FBH

FFH

00H

59H

42H

45H

54H

56H

02H

WORD_VAR

将上面

内容再

重复4次

答：DW 5150H

 

DB 50H, 51H

 

DB ‘PQ’

 

DW ‘QP’

 

ORG 5150H

DW $

4.7 请设置一个数据段DATASG，其中定义以下字符变量或数据变量。

(1) FLD1B为字符串变量：‘personal computer’；

(2) FLD2B为十进制数字节变量：32；

(3) FLD3B为十六进制数字节变量：20；

(4) FLD4B为二进制数字节变量：01011001；

(5) FLD5B为数字的ASCII字符字节变量：32654；

(6) FLD6B为10个零的字节变量；

(7) FLD7B为零件名(ASCII码)及其数量(十进制数)的表格：

PART1 20

PART2 50

PART3 14

(8) FLD1W为十六进制数字变量：FFF0；

(9) FLD2W为二进制数的字变量：01011001；

(10) FLD3W为(7)零件表的地址变量；

(11) FLD4W为包括5个十进制数的字变量：5，6，7，8，9；

(12) FLD5W为5个零的字变量；

(13) FLD6W为本段中字数据变量和字节数据变量之间的地址差。

答：DATASG SEGMENT

FLD1B DB ‘personal computer’

FLD2B DB 32

FLD3B DB 20H

FLD4B DB 01011001B

FLD5B DB ‘32654’

FLD6B DB 10 DUP (0)

FLD7B DB ‘PART1’, 20

DB ‘PART2’, 50

DB ‘PART3’, 14

FLD1W DW 0FFF0H

FLD2W DW 01011001B

FLD3W DW FLD7B

FLD4W DW 5, 6, 7, 8, 9

FLD5W DW 5 DUP (0)

FLD6W DW FLD1W-FLD1B

DATASG ENDS

4.8 假设程序中的数据定义如下：

PARTNO DW ?

PNAME DB 16 DUP (?)

COUNT DD ?

PLENTH EQU $-PARTNO

问PLENTH的值为多少？它表示什么意义？

答：PLENTH=22=16H，它表示变量PARTNO、PNAME、COUNT总共占用的存储单元数(字节数)。

4.9 有符号定义语句如下：

BUFF DB 1, 2, 3, ‘123’

EBUFF DB 0

L EQU EBUFF - BUFF

问L的值是多少？

答：L＝6。

4.10 假设程序中的数据定义如下：

LNAME DB 30 DUP (?)

ADDRESS DB 30 DUP (?)

CITY DB 15 DUP (?)

CODE_LIST DB 1, 7, 8, 3, 2

(1) 用一条MOV指令将LNAME的偏移地址放入AX。

(2) 用一条指令将CODE_LIST的头两个字节的内容放入SI。

(3) 用一条伪操作使CODE_LENGTH的值等于CODE_LIST域的实际长度。

答：(1) MOV AX, OFFSET LNAME

(2) MOV SI, WORD PTR CODE_LIST

(3) CODE_LENGTH EQU $ - CODE_LIST ；此语句必须放在CODE_LIST语句之后

4.11 试写出一个完整的数据段DATA_SEG，它把整数5赋予一个字节，并把整数-1，0，2，5和4放在10字数组DATA_LIST的头5个单元中。然后，写出完整的代码段，其功能为：把DATA_LIST中头5个数中的最大值和最小值分别存入MAX和MIN单元中。

答：DATA_SEG SEGMENT

NUM DB 5

DATA_LIST DW -1, 0, 2, 5, 4, 5 DUP (?)

MAX DW ?

MIN DW ?

DATA_SEG ENDS

；----------------------------------------------------------------

CODE_SEG SEGMENT

MAIN PROC FAR

ASSUME CS: CODE_SEG, DS: DATA_SEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DATA_SEG ；给DS赋值

MOV DS, AX

；

MOV CX, 4 ；程序段开始

LEA BX, DATA_LIST

MOV AX, [BX]

MOV MAX, AX

MOV MIN, AX

ROUT1: ADD BX, 2

MOV AX, [BX]

CMP AX, MAX

JNGE ROUT2

MOV MAX, AX

ROUT2: CMP AX, MIN

JNLE ROUT3

MOV MIN, AX

ROUT3: LOOP ROUT1 ；程序段结束

RET

MAIN ENDP

CODE_SEG ENDS

；----------------------------------------------------------------

END START

4.12 给出等值语句如下：

ALPHA EQU 100

BETA EQU 25

GAMMA EQU 2

下列表达式的值是多少？

(1) ALPHA * 100 + BETA ；=2729H

(2) ALPHA MOD GAMMA + BETA ；=19H

(3) (ALPHA +2) * BETA – 2 ；=9F4H

(4) (BETA / 3) MOD 5 ；=3H

(5) (ALPHA +3) * (BETA MOD GAMMA) ；=67H

(6) ALPHA GE GAMMA ；=0FFFFH

(7) BETA AND 7 ；=01H

(8) GAMMA OR 3 ；=03H

答：见注释。

4.13 对于下面的数据定义，三条MOV指令分别汇编成什么？(可用立即数方式表示)

TABLEA DW 10 DUP (?)

TABLEB DB 10 DUP (?)

TABLEC DB ‘1234’

┇

MOV AX, LENGTH TABLEA ；汇编成MOV AX, 000AH

MOV BL, LENGTH TABLEB ；汇编成MOV BL, 000AH

MOV CL, LENGTH TABLEC ；汇编成MOV CL, 0001H

答：见注释。

4.14 对于下面的数据定义，各条MOV指令单独执行后，有关寄存器的内容是什么？

FLDB DB ?

TABLEA DW 20 DUP (?)

TABLEB DB ‘ABCD’

(1) MOV AX, TYPE FLDB ；(AX)=0001H

(2) MOV AX, TYPE TABLEA ；(AX)=0002H

(3) MOV CX, LENGTH TABLEA ；(CX)=0014H

(4) MOV DX, SIZE TABLEA ；(DX)=0028H

(5) MOV CX, LENGTH TABLEB ；(CX)=0001H

答：见注释。

4.15 指出下列伪操作表达方式的错误，并改正之。

(1) DATA_SEG SEG ；DATA_SEG SEGMENT（伪操作错）

(2) SEGMENT ‘CODE’ ；SEGNAME SEGMENT ‘CODE’（缺少段名字）

(3) MYDATA SEGMENT/DATA ；MYDATA SEGMENT

┇

ENDS ；MYDATA ENDS（缺少段名字）

(4) MAIN_PROC PROC FAR ；删除END MAIN_PROC也可以

┇

END MAIN_PROC ；MAIN_PROC ENDP ；上下两句交换位置

MAIN_PROC ENDP ； END MAIN_PROC

答：见注释。

4.16 按下面的要求写出程序的框架

(1) 数据段的位置从0E000H开始，数据段中定义一个100字节的数组，其类型属性既是字又是字节；

(2) 堆栈段从小段开始，段组名为STACK；

(3) 代码段中指定段寄存器，指定主程序从1000H开始，给有关段寄存器赋值；

(4) 程序结束。

答：程序的框架如下：

DATA_SEG SEGMENT AT 0E000H

ARRAY_B LABEL BYTE

ARRAY_W DW 50 DUP (?)

DATA_SEG ENDS ；以上定义数据段

；----------------------------------------------------------------

STACK_SEG SEGMENT PARA STACK ‘STACK’

DW 100H DUP (?)

TOS LABEL WORD

STACK_SEG ENDS ；以上定义堆栈段

；----------------------------------------------------------------

CODE_SEG SEGMENT

MAIN PROC FAR

ASSUME CS: CODE_SEG, DS: DATA_SEG, SS: STACK_SEG

ORG 1000H

START: MOV AX, STACK_SEG

MOV SS, AX ；给SS赋值

MOV SP, OFFSET TOS ；给SP赋值

PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DATA_SEG

MOV DS, AX ；给DS赋值

┇ ；程序段部分

RET

MAIN ENDP

CODE_SEG ENDS ；以上定义代码段

；----------------------------------------------------------------

END START

4.17 写一个完整的程序放在代码段C_SEG中，要求把数据段D_SEG中的AUGEND和附加段E_SEG中的ADDEND相加，并把结果存放在D_SEG 段中的SUM中。其中AUGEND、ADDEND和SUM均为双精度数，AUGEND赋值为99251，ADDEND赋值为 -15962。

答：程序如下：

D_SEG SEGMENT

AUGW LABEL WORD

AUGEND DD 99251

SUM DD ?

D_SEG ENDS ；以上定义数据段

；----------------------------------------------------------------

E_SEG SEGMENT

ADDW LABEL WORD

ADDEND DD -15962

E_SEG ENDS ；以上定义附加段

；----------------------------------------------------------------

C_SEG SEGMENT

MAIN PROC FAR

ASSUME CS: C_SEG, DS: D_SEG, ES: E_SEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, D_SEG

MOV DS, AX ；给DS赋值

MOV AX, E_SEG

MOV ES, AX ；给ES赋值

；

MOV AX, AUGW ；以下6条指令进行加法计算

MOV BX, AUGW+2

ADD AX, ES: ADDW

ADC BX, ES: ADDW+2 ；不考虑有符号数溢出

MOV WORD PTR SUM, AX

MOV WORD PTR [SUM+2], BX

RET

MAIN ENDP

C_SEG ENDS ；以上定义代码段

；----------------------------------------------------------------

END START

4.18 请说明表示程序结束的微操作和结束程序执行的语句之间的差别。它们在源程序中应如何表示？

答：表示程序结束的微操作是指示汇编程序MASM结束汇编的标志，在源程序中用END表示；结束程序执行的语句是结束程序运行而返回操作系统的指令，在源程序中有多种表示方法，比如INT 20H或MOV AX, 4C00H INT 21H以及RET等。

4.19 试说明下述指令中哪些需要加上PTR操作符：

BVAL DB 10H，20H

WVAL DW 1000H

(1) MOV AL，BVAL ；不需要

(2) MOV DL，[BX] ；不需要

(3) SUB [BX]，2 ；需要，如SUB BYTE PTR [BX]，2

(4) MOV CL，WVAL ；需要，如MOV CL，BYTE PTR WVAL

(5) ADD AL，BVAL+1 ；不需要

答：见注释。

 

 



5.1 试编写一个汇编语言程序，要求对键盘输入的小写字母用大写字母显示出来。

答：程序段如下：

BEGIN: MOV AH, 1 ；从键盘输入一个字符的DOS调用

INT 21H

CMP AL, ‘a’ ；输入字符<‘a’吗？

JB STOP

CMP AL, ‘z’ ；输入字符>‘z’吗？

JA STOP

SUB AL, 20H ；转换为大写字母，用AND AL, 1101 1111B也可

MOV DL, AL ；显示一个字符的DOS调用

MOV AH, 2

INT 21H

JMP BEGIN

STOP: RET

5.2 编写程序，从键盘接收一个小写字母，然后找出它的前导字符和后续字符，再按顺序显示这三个字符。

答：程序段如下：

BEGIN: MOV AH, 1 ；从键盘输入一个字符的DOS调用

INT 21H

CMP AL, ‘a’ ；输入字符<‘a’吗？

JB STOP

CMP AL, ‘z’ ；输入字符>‘z’吗？

JA STOP

DEC AL ；得到前导字符

MOV DL, AL ；准备显示三个字符

MOV CX, 3

DISPLAY: MOV AH, 2 ；显示一个字符的DOS调用

INT 21H

INC DL

LOOP DISPLAY

STOP: RET

5.3 将AX寄存器中的16位数分成4组，每组4位，然后把这四组数分别放在AL、BL、CL和DL中。

答：程序段如下：

DSEG SEGMENT

STORE DB 4 DUP (?)

DSEG ENDS

┇

BEGIN: MOV CL, 4 ；右移四次

MOV CH, 4 ；循环四次

LEA BX, STORE

A10: MOV DX, AX

AND DX, 0FH ；取AX的低四位

MOV [BX], DL ；低四位存入STORE中

INC BX

SHR AX, CL ；右移四次

DEC CH

JNZ A10 ；循环四次完了码？

B10: MOV DL, STORE ；四组数分别放在AL、BL、CL和DL中

MOV CL, STORE+1

MOV BL, STORE+2

MOV AL, STORE+3

STOP: RET

5.4 试编写一程序，要求比较两个字符串STRING1和STRING2所含字符是否完全相同，若相同则显示‘MATCH’， 若不相同则显示‘NO MATCH’。

答：程序如下：

DSEG SEGMENT

STRING1 DB ‘I am a student.’

STRING2 DB ‘I am a student!’

YES DB ‘MATCH’, 0DH, 0AH, ‘$’

NO DB ‘NO MATCH’, 0DH, 0AH, ‘$’

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG, ES: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

MOV ES, AX ；给ES赋值

；

BEGIN: LEA SI, STRING1 ；设置串比较指令的初值

LEA DI, STRING2

CLD

MOV CX, STRING2 - STRING1

REPE CMPSB ；串比较

JNE DISPNO

LEA DX, YES ；显示MATCH

JMP DISPLAY

DISPNO: LEA DX, NO ；显示NO MATCH

DISPLAY: MOV AH, 9 ；显示一个字符串的DOS调用

INT 21H

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.5 试编写一程序，要求能从键盘接收一个个位数N，然后响铃N次(响铃的ASCII码为07)。

答：程序段如下：

BEGIN: MOV AH, 1 ；从键盘输入一个字符的DOS调用

INT 21H

SUB AL, ‘0’

JB STOP ；输入字符<‘0’吗？

CMP AL, 9 ；输入字符>‘9’吗？

JA STOP

CBW

MOV CX, AX ；响铃次数N

JCXZ STOP

BELL: MOV DL, 07H ；准备响铃

MOV AH, 2 ；显示一个字符的DOS调用，实际为响铃

INT 21H

CALL DELAY100ms ；延时100ms

LOOP BELL

STOP: RET

5.6 编写程序，将一个包含有20个数据的数组M分成两个数组：正数数组P和负数数组N，并分别把这两个数组中数据的个数显示出来。

答：程序如下：

DSEG SEGMENT

COUNT EQU 20

ARRAY DW 20 DUP (?) ；存放数组

COUNT1 DB 0 ；存放正数的个数

ARRAY1 DW 20 DUP (?) ；存放正数

COUNT2 DB 0 ；存放负数的个数

ARRAY2 DW 20 DUP (?) ；存放负数

ZHEN DB 0DH, 0AH, ‘The positive number is：’, ‘$’ ；正数的个数是：

FU DB 0DH, 0AH, ‘The negative number is：’, ‘$’ ；负数的个数是：

CRLF DB 0DH, 0AH, ‘$’

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV CX, COUNT

LEA BX, ARRAY

LEA SI, ARRAY1

LEA DI, ARRAY2

BEGIN1: MOV AX, [BX]

CMP AX, 0 ；是负数码？

JS FUSHU

MOV [SI], AX ；是正数，存入正数数组

INC COUNT1 ；正数个数+1

ADD SI, 2

JMP SHORT NEXT

FUSHU: MOV [DI], AX ；是负数，存入负数数组

INC COUNT2 ；负数个数+1

ADD DI, 2

NEXT: ADD BX, 2

LOOP BEGIN1

LEA DX, ZHEN ；显示正数个数

MOV AL, COUNT1

CALL DISPLAY ；调显示子程序

LEA DX, FU ；显示负数个数

MOV AL, COUNT2

CALL DISPLAY ；调显示子程序

RET

MAIN ENDP

；--------------------------------------------------------------------------

DISPLAY PROC NEAR ；显示子程序

MOV AH, 9 ；显示一个字符串的DOS调用

INT 21H

AAM ；将(AL)中的二进制数转换为二个非压缩BCD码

ADD AH, ‘0’ ；变为0～9的ASCII码

MOV DL, AH

MOV AH, 2 ；显示一个字符的DOS调用

INT 21H

ADD AL, ‘0’ ；变为0～9的ASCII码

MOV DL, AL

MOV AH, 2 ；显示一个字符的DOS调用

INT 21H

LEA DX, CRLF ；显示回车换行

MOV AH, 9 ；显示一个字符串的DOS调用

INT 21H

RET

DISPLAY ENDP ；显示子程序结束

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.7 试编写一个汇编语言程序，求出首地址为DATA的100D字数组中的最小偶数，并把它存放在AX中。

答：程序段如下：

BEGIN: MOV BX, 0

MOV CX, 100

COMPARE: MOV AX, DATA[BX] ；取数组的第一个偶数

ADD BX, 2

TEST AX, 01H ；是偶数吗？

LOOPNZ COMPARE ；不是，比较下一个数

JNZ STOP ；没有偶数，退出

JCXZ STOP ；最后一个数是偶数，即为最小偶数，退出

COMPARE1: MOV DX, DATA[BX] ；取数组的下一个偶数

ADD BX, 2

TEST DX, 01H ；是偶数吗？

JNZ NEXT ；不是，比较下一个数

CMP AX, DX ；(AX)<(DX)吗？

JLE NEXT

MOV AX, DX ；(AX)<(DX)，则置换(AX)为最小偶数

NEXT: LOOP COMPARE1

STOP: RET

5.8 把AX中存放的16位二进制数K看作是8个二进制的“四分之一字节”。试编写程序要求数一下值为3(即11B)的四分之一字节数，并将该数(即11B的个数)在终端上显示出来。

答：程序段如下：

BEGIN: MOV DL, 0 ；计数初始值

MOV CX, 8

COMPARE: TEST AX, 03H ；是数03吗？

JNZ NOEQUAL ；不是，转走

INC DL ；是，计数

NOEQUAL: ROR AX, 1 ；准备判断下一个数

ROR AX, 1

LOOP COMPARE

ADD DL, ‘0’ ；将计数值转换为ASCII码

MOV AH, 2 ；进行显示

INT 21H

STOP: RET

5.9 试编写一个汇编语言程序，要求从键盘接收一个四位的16进制数，并在终端上显示与它等值的二进制数。

答：程序段如下：

BEGIN: MOV BX, 0 ；用于存放四位的16进制数

MOV CH, 4

MOV CL, 4

INPUT: SHL BX, CL ；将前面输入的数左移4位

MOV AH, 1 ；从键盘取数

INT 21H

CMP AL, 30H ；<0吗？

JB INPUT ；不是‘0～F’的数重新输入

CMP AL, 39H ；是‘0～9’吗？

JA AF ；不是，转‘A～F’的处理

AND AL, 0FH ；转换为：0000B～1001B

JMP BINARY

AF: AND AL, 1101 1111B ；转换为大写字母

CMP AL, 41H ；又<A吗？

JB INPUT ；不是‘A～F’的数重新输入

CMP AL, 46H ；>F吗？

JA INPUT ；不是‘A～F’的数重新输入

AND AL, 0FH ；转换为：1010B～1111B

ADD AL, 9

BINARY: OR BL, AL ；将键盘输入的数进行组合

DEL CH

JNZ INPUT

DISPN: MOV CX, 16 ；将16位二进制数一位位地转换成ASCII码显示

DISP: MOV DL, 0

ROL BX, 1

RCL DL, 1

OR DL, 30H

MOV AH, 2 ；进行显示

INT 21H

LOOP DISP

STOP: RET

5.10 设有一段英文，其字符变量名为ENG，并以$字符结束。试编写一程序，查对单词SUN在该文中的出现次数，并以格式“SUN：xxxx”显示出次数。

答：程序如下：

DSEG SEGMENT

ENG DB ‘Here is sun, sun ,…,$’

DISP DB ‘SUN：’

DAT DB ‘0000’ , 0DH, 0AH, ‘$’

KEYWORD DB ‘sun’

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG, ES: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

MOV ES, AX ；给ES赋值

BEGIN: MOV AX, 0

MOV DX, DISP-ENG-2 ；计算ENG的长度(每次比较sun,因此比较次数-2)

LEA BX, ENG

COMP: MOV DI, BX

LEA SI, KEYWORD

MOV CX, 3

REPE CMPSB ；串比较

JNZ NOMATCH

INC AX ；是，SUN的个数加1

ADD BX, 2

NOMATCH: INC BX ；指向ENG的下一个字母

DEC DX

JNZ COMP

DONE: MOV CH, 4 ；将次数转换为16进制数的ASCII码

MOV CL, 4

LEA BX, DAT ；转换结果存入DAT单元中

DONE1: ROL AX, CL

MOV DX, AX

AND DL, 0FH ；取一位16进制数

ADD DL, 30H

CMP DL, 39H

JLE STORE

ADD DL, 07H ；是“A～F”所以要加7

STORE: MOV [BX], DL ；转换结果存入DAT单元中

INC BX

DEC CH

JNZ DONE1

DISPLAY: LEA DX, DISP ；显示字符串程序(将DISP和DAT一起显示)

MOV AH, 09H

INT 21H

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.11 从键盘输入一系列以$为结束符的字符串，然后对其中的非数字字符计数，并显示出计数结果。

答：程序段如下：

DSEG SEGMENT

BUFF DB 50 DUP (‘ ’)

COUNT DW 0

DSEG ENDS

┇

BEGIN: LEA BX, BUFF

MOV COUNT, 0

INPUT: MOV AH, 01 ；从键盘输入一个字符的功能调用

INT 21H

MOV [BX], AL

INC BX

CMP AL, ‘$’ ；是$结束符吗？

JNZ INPUT ；不是，继续输入

LEA BX, BUFF ；对非数字字符进行计数

NEXT: MOV CL, [BX]

INC BX

CMP CL, ‘$’ ；是$结束符，则转去显示

JZ DISP

CMP CL, 30H ；小于0是非数字字符

JB NEXT

CMP CL, 39H ；大于9是非数字字符

JA NEXT

INC COUNT ；个数+1

JMP NEXT

DISP: ┇ ；16进制数显示程序段(省略)

5.12 有一个首地址为MEM的100D字数组，试编制程序删除数组中所有为0的项，并将后续项向前压缩，最后将数组的剩余部分补上0。

答：程序如下：

DSEG SEGMENT

MEM DW 100 DUP (?)

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV SI, (100-1)*2 ；(SI)指向MEM的末元素的首地址

MOV BX, -2 ；地址指针的初值

MOV CX, 100

COMP: ADD BX, 2

CMP MEM [BX], 0

JZ CONS

LOOP COMP

JMP FINISH ；比较完了，已无0则结束

CONS: MOV DI, BX

CONS1: CMP DI, SI ；到了最后单元码？

JAE NOMOV

MOV AX, MEM [DI+2] ；后面的元素向前移位

MOV MEM [DI], AX

ADD DI, 2

JMP CONS1

NOMOV: MOV WORD PTR [SI], 0 ；最后单元补0

LOOP COMP

FINISH: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.13 在STRING到STRING+99单元中存放着一个字符串，试编制一个程序测试该字符串中是否存在数字，如有则把CL的第5位置1，否则将该位置0。

答：程序如下：

DSEG SEGMENT

STRING DB 100 DUP (?)

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV SI, 0 ；(SI)作为地址指针的变化值

MOV CX, 100

REPEAT: MOV AL, STRING [SI]

CMP AL, 30H

JB GO_ON

CMP AL, 39H

JA GO_ON

OR CL, 20H ；存在数字把CL的第5位置1

JMP EXIT

GO_ON: INC SI

LOOP REPEAT

AND CL, 0DFH ；不存在数字把CL的第5位置0

EXIT: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.14 在首地址为TABLE的数组中按递增次序存放着100H个16位补码数，试编写一个程序把出现次数最多的数及其出现次数分别存放于AX和CX中。

答：程序如下：

DSEG SEGMENT

TABLE DW 100H DUP (?) ；数组中的数据是按增序排列的

DATA DW ?

COUNT DW 0

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV CX, 100H ；循环计数器

MOV SI, 0

NEXT: MOV DX, 0

MOV AX, TABLE [SI]

COMP: CMP TABLE [SI], AX ；计算一个数的出现次数

JNE ADDR

INC DX

ADD SI, 2

LOOP COMP

ADDR: CMP DX, COUNT ；此数出现的次数最多吗？

JLE DONE

MOV COUNT, DX ；目前此数出现的次数最多，记下次数

MOV DATA, AX ；记下此数

DONE: LOOP NEXT ；准备取下一个数

MOV CX, COUNT ；出现最多的次数存入(CX)

MOV AX, DATA ；出现最多的数存入(AX)

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.15 数据段中已定义了一个有n个字数据的数组M，试编写一程序求出M中绝对值最大的数，把它放在数据段的M+2n单元中，并将该数的偏移地址存放在M+2(n+1)单元中。

答：程序如下：

DSEG SEGMENT

n EQU 100H ；假设n=100H

M DW n DUP (?)

DATA DW ? ；M+2n单元

ADDR DW ? ；M+2(n+1)单元

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV CX, n ；循环计数器

LEA DI, M

MOV AX, [DI] ；取第一个数

MOV ADDR, DI ；记下绝对值最大的数的地址

CMP AX, 0 ；此数是正数吗？

JNS ZHEN ；是正数，即为绝对值，转去判断下一个数

NEG AX ；不是正数，变为其绝对值

ZHEN: MOV BX, [DI]

CMP BX, 0 ；此数是正数吗？

JNS COMP ；是正数，即为绝对值，转去比较绝对值大小

NEG BX ；不是正数，变为其绝对值

COMP: CMP AX, BX ；判断绝对值大小

JAE ADDRESS

MOV AX, BX ；(AX)<(BX)，使(AX)中为绝对值最大的数

MOV ADDR, DI ；记下绝对值最大的数的地址

ADDRESS: ADD DI, 2

LOOP ZHEN

MOV DATA, AX ；记下此数

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.16 在首地址为DATA的字数组中存放着100H个16位补码数，试编写一个程序求出它们的平均值放在AX寄存器中；并求出数组中有多少个数小于此平均值，将结果放在BX寄存器中。

答：程序如下：

DSEG SEGMENT

DATA DW 100H DUP (?)

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV CX, 100H ；循环计数器

MOV SI, 0

MOV BX, 0 ；和((DI),(BX))的初始值

MOV DI, 0

NEXT: MOV AX, DATA [SI]

CWD

ADD BX, AX ；求和

ADC DI, DX ；加上进位位

ADD SI, 2

LOOP NEXT

MOV DX, DI ；将((DI),(BX))中的累加和放入((DX),(AX))中

MOV AX, BX

MOV CX, 100H

IDIV CX ；带符号数求平均值，放入(AX)中

MOV BX, 0

MOV SI, 0

COMP: CMP AX, DATA [SI] ；寻找小于平均值的数

JLE NO

INC BX ；小于平均值数的个数+1

NO: ADD SI, 2

LOOP COMP

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.17 试编制一个程序把AX中的16进制数转换为ASCII码，并将对应的ASCII码依次存放到MEM数组中的四个字节中。例如，当(AX)=2A49H时，程序执行完后，MEM中的4个字节内容为39H，34H，41H，32H。

答：程序如下：

DSEG SEGMENT

MEM DB 4 DUP (?)

N DW 2A49H

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV CH, 4 ；循环计数器

MOV CL, 4

MOV AX, N

LEA BX, MEM

ROTATE: MOV DL, AL ；从最低四位开始转换为ASCII码

AND DL, 0FH

ADD DL, 30H

CMP DL, 3AH ；是0~9吗？

JL NEXT

ADD DL, 07H ；是A~F

NEXT: MOV [BX], DL ；转换的ASCII码送入MEM中

INC BX

ROR AX, CL ；准备转换下一位

DEC CH

JNZ ROTATE

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.18 把0~100D之间的30个数存入以GRADE为首地址的30字数组中，GRADE+i表示学号为i+1的学生的成绩。另一个数组RANK为30个学生的名次表，其中RANK+i的内容是学号为i+1的学生的名次。编写一程序，根据GRADE中的学生成绩，将学生名次填入RANK数组中。(提示：一个学生的名次等于成绩高于这个学生的人数加1。)

答：程序如下：

DSEG SEGMENT

GRADE DW 30 DUP (?) ；假设已预先存好30名学生的成绩

RANK DW 30 DUP (?)

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV DI, 0

MOV CX, 30 ；外循环计数器

LOOP1: PUSH CX

MOV CX, 30 ；内循环计数器

MOV SI, 0

MOV AX, GRADE [DI]

MOV DX, 1 ；起始名次为第1名

LOOP2: CMP GRADE [SI], AX ；成绩比较

JBE GO_ON

INC DX ；名次+1

GO_ON: ADD SI, 2

LOOP LOOP2

POP CX

MOV RNAK [DI], DX ；名次存入RANK数组

ADD DI, 2

LOOP LOOP1

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.19 已知数组A包含15个互不相等的整数，数组B包含20个互不相等的整数。试编制一程序把既在A中又在B中出现的整数存放于数组C中。

答：程序如下：

DSEG SEGMENT

A DW 15 DUP (?)

B DW 20 DUP (?)

C DW 15 DUP (‘ ’)

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV SI, 0

MOV BX, 0

MOV CX, 15 ；外循环计数器

LOOP1: PUSH CX

MOV CX, 20 ；内循环计数器

MOV DI, 0

MOV AX, A [SI] ；取A数组中的一个数

LOOP2: CMP B [DI], AX ；和B数组中的数相等吗？

JNE NO

MOV C [BX], AX ；相等存入C数组中

ADD BX, 2

NO: ADD DI, 2

LOOP LOOP2

ADD SI, 2

POP CX

LOOP LOOP1

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.20 设在A、B和C单元中分别存放着三个数。若三个数都不是0，则求出三数之和存放在D单元中；若其中有一个数为0，则把其它两单元也清0。请编写此程序。

答：程序如下：

DSEG SEGMENT

A DW ?

B DW ?

C DW ?

D DW 0

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: CMP A, 0

JE NEXT

CMP B, 0

JE NEXT

CMP C, 0

JE NEXT

MOV AX, A

ADD AX, B

ADD AX, C

MOV D, AX

JMP SHORT EXIT

NEXT: MOV A, 0

MOV B, 0

MOV C, 0

EXIT: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.21 试编写一程序，要求比较数组ARRAY中的三个16位补码数，并根据比较结果在终端上显示如下信息：

(1) 如果三个数都不相等则显示0；

(2) 如果三个数有二个数相等则显示1；

(3) 如果三个数都相等则显示2。

答：程序如下：

DSEG SEGMENT

ARRAY DW 3 DUP (?)

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: LEA SI, ARRAY

MOV DX, 0 ；(DX)用于存放所求的结果

MOV AX, [SI]

MOV BX, [SI+2]

CMP AX, BX ；比较第一和第二两个数是否相等

JNE NEXT1

INC DX

NEXT1: CMP [SI+4], AX ；比较第一和第三两个数是否相等

JNE NEXT2

INC DX

NEXT2: CMP [SI+4], BX ；比较第二和第三两个数是否相等

JNE NUM

INC DX

NUM: CMP DX, 3

JL DISP

DEC DX

DISP: ADD DL, 30H ；转换为ASCII码

MOV AH, 2 ；显示一个字符

INT 21H

RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.22 从键盘输入一系列字符(以回车符结束)，并按字母、数字、及其它字符分类计数，最后显示出这三类的计数结果。

答：程序如下：

DSEG SEGMENT

ALPHABET DB ‘输入的字母字符个数为：’, ‘$’

NUMBER DB ‘输入的数字字符个数为：’, ‘$’

OTHER DB ‘输入的其它字符个数为：’, ‘$’

CRLF DB 0DH, 0AH, ‘$’

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV BX, 0 ；字母字符计数器

MOV SI, 0 ；数字字符计数器

MOV DI, 0 ；其它字符计数器

INPUT: MOV AH, 1 ；输入一个字符

INT 21H

CMP AL, 0DH ；是回车符吗？

JE DISP

CMP AL, 30H ；<数字0吗？

JAE NEXT1

OTHER: INC DI ；是其它字符

JMP SHORT INPUT

NEXT1: CMP AL, 39H ；>数字9吗？

JA NEXT2

INC SI ；是数字字符

JMP SHORT INPUT

NEXT2: CMP AL, 41H ；<字母A吗？

JAE NEXT3

JMP SHORT OTHER ；是其它字符

NEXT3: CMP AL, 5AH ；>字母Z吗？

JA NEXT4

INC BX ；是字母字符A~Z

JMP SHORT INPUT

NEXT4: CMP AL, 61H ；<字母a吗？

JAE NEXT5

JMP SHORT OTHER ；是其它字符

NEXT5: CMP AL, 7AH ；>字母z吗？

JA SHORT OTHER ；是其它字符

INC BX ；是字母字符a~z

JMP SHORT INPUT

 

DISP: LEA DX, ALPHABET

CALL DISPLAY

LEA DX, NUMBER

MOV BX, SI

CALL DISPLAY

LEA DX, OTHER

MOV BX, DI

CALL DISPLAY

RET

MAIN ENDP

；--------------------------------------------------------------------------

DISPLAY PROC NEAR

MOV AH, 09H ；显示字符串功能调用

INT 21H

CALL BINIHEX ；调把BX中二进制数转换为16进制显示子程序

LEA DX, CRLF

MOV AH, 09H ；显示回车换行

INT 21H

RET

DISPLAY ENDP

；--------------------------------------------------------------------------

BINIHEX PROC NEAR ；将BX中二进制数转换为16进制数显示子程序

MOV CH, 4

ROTATE: MOV CL, 4

ROL BX, CL

MOV DL, BL

AND DL, 0FH

ADD DL, 30H

CMP DL, 3AH ；是A~F吗？

JL PRINT_IT

ADD DL, 07H

PRINT_IT: MOV AH, 02H ；显示一个字符

INT 21H

DEC CH

JNZ ROTATE

RET

BINIHEX ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.23 已定义了两个整数变量A和B，试编写程序完成下列功能：

(1) 若两个数中有一个是奇数，则将奇数存入A中，偶数存入B中；

(2) 若两个数中均为奇数，则将两数加1后存回原变量；

(3) 若两个数中均为偶数，则两个变量均不改变。

答：程序如下：

DSEG SEGMENT

A DW ？

B DW ？

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV AX, A

MOV BX, B

XOR AX, BX

TEST AX, 0001H ；A和B同为奇数或偶数吗？

JZ CLASS ；A和B都为奇数或偶数，转走

TEST BX, 0001H

JZ EXIT ；B为偶数，转走

XCHG BX, A ；A为偶数，将奇数存入A中

MOV B, BX ；将偶数存入B中

JMP EXIT

CLASS: TEST BX, 0001H ；A和B都为奇数吗？

JZ EXIT ；A和B同为偶数，转走

INC B

INC A

EXIT: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.24 假设已编制好5个歌曲程序，它们的段地址和偏移地址存放在数据段的跳跃表SINGLIST中。试编制一程序，根据从键盘输入的歌曲编号1~5，转去执行五个歌曲程序中的某一个。

答：程序如下：

DSEG SEGMENT

SINGLIST DD SING1

DD SING2

DD SING3

DD SING4

DD SING5

ERRMSG DB ‘Error! Invalid parameter!’, 0DH, 0AH, ‘$’

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV AH, 1 ；从键盘输入的歌曲编号1~5

INT 21H

CMP AL, 0DH

JZ EXIT ；是回车符，则结束

SUB AL, ‘1’ ；是1~5吗？

JB ERROR ；小于1，错误

CMP AL, 4

JA ERROR ；大于5，错误

MOV BX, OFFSET SINGLIST

MUL AX, 4 ；(AX)=(AL)*4,每个歌曲程序的首地址占4个字节

ADD BX, AX

JMP DWORD PTR[BX] ；转去执行歌曲程序

ERROR: MOV DX, OFFSET ERRMSG

MOV AH, 09H

INT 21H ；显示错误信息

JMP BEGIN

SING1: ┇

JMP BEGIN

SING2: ┇

JMP BEGIN

SING3: ┇

JMP BEGIN

SING4: ┇

JMP BEGIN

SING5: ┇

JMP BEGIN

EXIT: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

5.25 试用8086的乘法指令编制一个32位数和16位数相乘的程序；再用80386的乘法指令编制一个32位数和16位数相乘的程序，并定性比较两个程序的效率。

答：8086的程序如下(假设为无符号数)：

DSEG SEGMENT

MUL1 DD ? ；32位被乘数

MUL2 DW ? ；16位乘数

MUL0 DW 0，0 ，0 ，0 ；乘积用64位单元存放

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOV BX, MUL2 ；取乘数

MOV AX, WORD PTR MUL1 ；取被乘数低位字

MUL BX

MOV MUL0, AX ；保存部分积低位

MOV MUL0+2, DX ；保存部分积高位

MOV AX, WORD PTR[MUL1+2] ；取被乘数高位字

MUL BX

ADD MUL0+2, AX ；部分积低位和原部分积高位相加

ADC MUL0+4, DX ；保存部分积最高位，并加上进位

EXIT: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

80386的程序如下(假设为无符号数)：

.386

DSEG SEGMENT

MUL1 DD ? ；32位被乘数

MUL2 DW ? ；16位乘数

MUL0 DD 0，0 ；乘积用64位单元存放

DSEG ENDS

；--------------------------------------------------------------------------

CSEG SEGMENT

MAIN PROC FAR

ASSUME CS: CSEG, DS: DSEG

START: PUSH DS ；设置返回DOS

SUB AX, AX

PUSH AX

MOV AX, DSEG

MOV DS, AX ；给DS赋值

 

BEGIN: MOVZX EBX, MUL2 ；取乘数，并0扩展成32位

MOV EAX, MUL1 ；取被乘数

MUL EBX

MOV DWORD PTR MUL0, EAX ；保存积的低位双字

MOV DWORD PTR[MUL0+4], EDX ；保存积的高位双字

EXIT: RET

MAIN ENDP

CSEG ENDS ；以上定义代码段

；--------------------------------------------------------------------------

END START

80386作32位乘法运算用一条指令即可完成，而8086则需用部分积作两次完成。

5.26 如数据段中在首地址为MESS1的数据区内存放着一个长度为35的字符串，要求把它们传送到附加段中的缓冲区MESS2中去。为提高程序执行效率，希望主要采用MOVSD指令来实现。试编写这一程序。

答：80386的程序如下：

.386

.MODEL SMALL

.STACK 100H

.DATA

MESS1 DB ‘123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ’,? ；长度为35的字符串

.FARDATA

MESS2 DB 36 DUP (?)

.CODE

START: MOV AX, @DATA

MOV DS, AX ；给DS赋值

MOV AX, @FARDATA

MOV ES, AX ；给ES赋值

ASSUME ES:@FARDATA

 

BEGIN: LEA ESI, MESS1

LEA EDI, MESS2

CLD

MOV ECX, (35+1)/4 ；取传送的次数

REP MOVSD

；--------------------------------------------------------------------------

MOV AX, 4C00H ；返回DOS

INT 21H

END START

5.27 试用比例变址寻址方式编写一386程序，要求把两个64位整数相加并保存结果。

答：80386的程序如下：

.386

.MODEL SMALL

.STACK 100H

.DATA

DATA1 DQ ?

DATA2 DQ ?

.CODE

START: MOV AX, @DATA

MOV DS, AX ；给DS赋值

BEGIN: MOV ESI, 0

MOV EAX, DWORD PTR DATA2[ESI*4]

ADD DWORD PTR DATA1[ESI*4], EAX

INC ESI

MOV EAX, DWORD PTR DATA2[ESI*4]

ADC DWORD PTR DATA1[ESI*4], EAX

；--------------------------------------------------------------------------

MOV AX, 4C00H ；返回DOS

INT 21H

END START