---
title: G++基础
tags: [G++,C++]
date: 2022/7/14
categories: GCC/G++
comments: false
---



## G++/GCC基础

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com/img/gcc.png)



### 编译过程有四步

1. #### 预处理

   ```bash
   g++ -E test.cpp -o test.i
   #预处理为.i文件
   #头文件啥的的预定义啥的
   ```

   

2. #### 编译

   ```bash
   g++ -S test.i -o test.s
   #编译为汇编语言输出为.s文件
   ```

   

3. #### 汇编

   ```bash
   g++ -c test.s -o test.o
   #编译为机器语言输出为.o
   ```

   

4. #### 链接

   ```bash
   g++ test.o test
   #生成为可执行文件test，二进制的文件
   ```



### GCC/G++的优化选项

```bash
#基本优化
-O 
#等效O1
-O0
#不做优化
-O1
#为默认优化
-O2
#默认优化+额外的调整
-O3
#默认优化+额外的调整+循环展开等处理特性的优化

g++ test.cpp -O2 -o test
#优化等级2生成可执行文件test
```

所谓优化就是提升效率，编译时间换取执行效率

示例:

```c++

#include <iostream>
using	namespace std;

int main ()
{

	unsigned long int counter;
	unsigned long int result;
	unsigned long int temp;
	unsigned int five;
	int i;
	
	//有很多计算步骤都都在循环条件里
	for(counter=0;counter<2000*2000*100/4+2010;counter+=(10-6)/4)
	{
	
		temp+=counter/1979;
		for(i=0;i<20;i++)
		{
			five=200*200/8000;
			result=counter;
		}
	}
	cout<<"result="<<result<<endl;
	return 0;
}


```

测试优化O2与不优化O0

```bash
g++ test.cpp -O2 -o Youhua
```

```bash
g++ test.cpp -O0 -o NotYouhua
```

再依次执行

```bash
time ./Youhua
time ./NotYouhua
```



可以看见优化后的执行效率相当高

### 指定库文件与头文件

库的指定：

```bash
g++ -lglog test.cpp
#链接golg 库

g++ -L/home/icrad/mylibfolder -lmylib test.cpp
#链接自己的指定的库文件夹下的库文件，需要大写
```

头文件的指定



```bash
#一般来说，是不需要指定的，GCC默认去找include文件夹，当不存在时，就需要自己指定了头文件目录了

g++ -I/myinclude test.cpp
```



### 基本常用的其他项

-  -Wall

   打印警告信息

  ```bash
  g++ -Wall test.cpp
  ```

  

- -w

  关闭警告信息

  ```bash
  g++ -W test.cpp
  ```

  

- -std=c++11

  指定C++ 特性

  ```bash
  g++ -std=c++11 test.cpp
  ```

  

- -o

  编译输出可执行文件

  ```bash
  g++ test.cpp -o test
  ```

  

- -D

  定义宏

  ```c++
  //使用场景，可以在编译的时候选择是否 执行宏定义
  #include <iostream>
  using namespace std;
  int main ()
  {
      #ifdef DEBUG
       cout<<"DEBUG_LOG"<<endl;
      #endif
      cout<<"Ubuntu,yes!";
      return 0;
  }
  
  ```

  编译这个文件时可以

  ```bash
  g++ -DDEBUG test.cpp -o test
  ```

  生成的执行文件会将宏定义中的代码输出

  如果没有定义宏，只会输出

  ```
    Ubuntu,yes!
  ```

  

### GDB调试参数

调试需要在编译时加入可调试信息才可使用GDB工具进行调试

```bash
g++ -g test.cpp -o test
#生成可调试的执行文件
```



### 编译示例



#### 多文件夹的目录结构

```
├── include
│   └── swap.h
├── math.cpp
└── src
    └── swap.cpp
```

- #### 直接编译

  ```bash
  g++ math.cpp -std=c++11 src/swap.cpp -Iinclude
  #记得引入头文件
  ```

  即可生成可执行文件a.out 



- #### 链接生成静态库

  ```bash
  #将src的文件夹下的swap.cpp生成为.o文件(汇编语言)
  cd src/
  g++ swap.cpp  -c -I../include
  
  #进行归档操作
  ar rs libswap.a swap.o
  #作用是将swap.o归档为一个静态库文件
  
  cd ..#回到上级目录
  g++ math.cpp -lswap -Lsrc -I\include -o static_swap
  #链接指定的静态库进行编译
  #最终得到可执行文件static_swap
  ```

### 

- #### 链接生成动态库

  ```bash
  cd src/
  
  g++ swap.cpp -I../include -fPIC -shared -o libswap.so
  #等价于
  gcc	swap.cpp -I../include -c -fPIC 
  #生成.o文件
  gcc -shared -o libswap.so swap.o
  #生成静态库文件libswap.so
  
  回到上级目录
  cd ..
  g++ math .cpp -Iinclude -lswap -Lsrc -o dyna_swap
  #生成了可执行的文件dyna_swap
  
  ```

  动态库的可执行文件需要指定搜索路径

  ```bash
  LD_LIBRARY_PATH=src ./dyna_swap
  #指定了动态库的搜索路径运行该可执行文件
  ```

  



静态库与静态库的区别：大小，前者更大，动态库在调用相应函数时才会加载，所以在运行时，静态库不需要指定路径直接运行，而动态库需要。

