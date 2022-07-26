---
title: GDB基础
tags: [GDB,C++,调试]
date: 2022/7/14
categories: GDB
comments: false
---



## GDB的调试

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com/img/gdb.jpg)

**GDB (GUN DEBUGGER)**

1. 设置断点

2. 单步运行

3. 查看变量值

4. 动态改变执行环境

5. 分析崩溃产生core文件

   

### GDB调试参数

调试需要在编译时加入可调试信息才可使用GDB工具进行调试

```bash
g++ -g test.cpp -o test
#生成可调试的执行文件
```



### GDB 常用指令

​	说多也不多，毕竟现在调试大多都是gui调试，暂时很少直接用gdb命令行进行调试，写几个比较基础的~~糊弄糊弄，就记住几个~~

```bash
gdb [file]
#执行gdb调试

#进入gdb，可以直接输入命令

break(b) [num]
#在第num行代码设置断电

info breakpoint 
#查看当前代码信息

display [变量]
#显示变量，在每次运行到断点显示值

continue
#继续运行

run
#运行程序

quit
#退出GDB调试

#更多的我也记不到了，用men gdb指令查吧
```



