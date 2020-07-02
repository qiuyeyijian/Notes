## GCC和GDB

预处理、编译、汇编、连接

```bash
gcc -E test.c -o test.i				#预处理
gcc -S test.i -o test.s				#编译
gcc -c test.s -o test.o				#汇编
gcc test.o -o test					#链接
```

编译生成可调式文件

```c
gcc -g test.c -o test
```

使用GDB调试

```bash
gdb test  # 启动GDB
(gdb) list  # 从第一行列出源码
(gdb)       # 直接回车表示重复上一次命令
(gdb) break 16  # 设置断点，在源程序第16行处
(gdb) break func  # 设置断点，在函数func()入口处
(gdb) info break    # 查看断点信息
(gdb) run      # 运行程序
(gdb) next    # 单条语句执行
(gdb) n       # 单条语句执行，与next一样
(gdb) continue    # 继续运行程序
(gdb) print i    # 打印变量 i 的值
(gdb) p sum    # 打印变量sum的值
(gdb) bt    # 查看函数堆栈
(gdb) finish  # 退出函数
(gdb) quit    # 退出gdb
(gdb) step  # 单步调试，可以进入函数内部
```







