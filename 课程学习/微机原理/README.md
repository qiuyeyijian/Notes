```assembly
STACK1  SEGMENT   'STACK'
STT  DB        100 DUP( )
STACK1  ENDS
TOP  SEGMENT   'TOP'
TOP  ENDS

DATA  SEGMENT   'DATA'
STR1  DB        0F2H,0F6H,0F4H,0FCH,0F8H,0F9H,0F1H,0F3H         ;控制数据表  正转1档
STR2  DB        0E3H,0E1H,0E9H,0E8H,0ECH,0E4H,0E6H,0E2H         ;控制数据表  反转1档
STR3  DB        0D2H,0D6H,0D4H,0DCH,0D8H,0D9H,0D1H,0D3H         ;控制数据表   正转2档
STR4  DB        0C3H,0C3H,0C9H,0C8H,0CCH,0C4H,0C6H,0C2H         ;控制数据表  反转2档
STR5  DB        92H,96H,94H,9CH,98H,99H,91H,93H     ;控制数据表 正转3档
STR6  DB        83H,81H,89H,88H,8CH,84H,86H,82H     ;控制数据表 反转3档
DATA  ENDS

CODE  SEGMENT   'CODE'
ASSUME    CS:CODE,SS:STACK1,DS:DATA

IOCON  EQU       8006H
IOA  EQU       8000H
IOB  EQU       8002H
IOC  EQU       8004H

START:
MOV       AX, DATA
MOV       DS, AX
MOV       AX, STACK1
MOV       SS, AX
MOV       AX, TOP
MOV       SP, AX
MOV       DX,IOCON
MOV       AL,90H
OUT       DX,AL
MOV       AL,0FFH

MOT23:  MOV       CX,08H
LEA       DI,STR6
IOLED23:  MOV       AL,[DI]
MOV       DX,IOB
OUT       DX,AL
MOV       DX,IOA
IN        AL,DX

TEST      AL,01H
JE        MOT11
TEST      AL,02H
JE        MOT21
TEST      AL,04H
JE        MOT22

INC       DI
CALL      DELAY1
LOOP      IOLED23
JMP       MOT23

MOT22:  MOV       CX,08H
LEA       DI,STR4
IOLED22:  MOV       AL,[DI]
MOV       DX,IOB
OUT       DX,AL
MOV       DX,IOA
IN        AL,DX

TEST      AL,01H
JE        MOT11
TEST      AL,02H
JE        MOT21
TEST      AL,08H
JE        MOT23

INC       DI
CALL      DELAY2
LOOP      IOLED22
JMP       MOT22

MOT21:  MOV       CX,08H
LEA       DI,STR2
IOLED21:  MOV       AL,[DI]
MOV       DX,IOB
OUT       DX,AL
MOV       DX,IOA
IN        AL,DX

TEST      AL,01H
JE        MOT11
TEST      AL,04H
JE        MOT22
TEST      AL,08H
JE        MOT23

INC       DI
CALL      DELAY3
LOOP      IOLED21
JMP       MOT21

MOT11:  MOV       CX,08H
LEA       DI,STR1
IOLED11:  MOV       AL,[DI]
MOV       DX,IOB
OUT       DX,AL
MOV       DX,IOA
IN        AL,DX

TEST      AL,02H
JE        MOT21       ; 为0
TEST      AL,04H
JE        MOT12
TEST      AL,08H
JE        MOT13

INC       DI
CALL      DELAY3
LOOP      IOLED11
JMP       MOT11

MOT12:  MOV       CX,08H
LEA       DI,STR3
IOLED12:  MOV       AL,[DI]
MOV       DX,IOB
OUT       DX,AL
MOV       DX,IOA
IN        AL,DX

TEST      AL,02H
JE        MOT21       ; 为0
TEST      AL,01H
JE        MOT11
TEST      AL,08H
JE        MOT13

INC       DI
CALL      DELAY2
LOOP      IOLED12
JMP       MOT12

MOT13:  MOV       CX,08H
LEA       DI,STR5
IOLED13:  MOV       AL,[DI]
MOV       DX,IOB
OUT       DX,AL
MOV       DX,IOA
IN        AL,DX

TEST      AL,02H
JE        MOT21       ; 为0
TEST      AL,01H
JE        MOT11
TEST      AL,04H
JE        MOT12

INC       DI
CALL      DELAY1
LOOP      IOLED13
JMP       MOT13

DELAY1:  PUSH      CX
MOV       CX,2000H
DELAY11:  NOP
NOP
NOP
NOP
LOOP      DELAY11
POP       CX
RET

DELAY2:  PUSH      CX
MOV       CX,4000H
DELAY21:  NOP
NOP
NOP
NOP
LOOP      DELAY21
POP       CX
RET

DELAY3:  PUSH      CX
MOV       CX,7000H
DELAY31:  NOP
NOP
NOP
NOP
LOOP      DELAY31
POP       CX
RET

CODE  ENDS

END       START
```



这是电动小车电机控制写入8086中的代码。

