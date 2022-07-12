---
title: 利用VScode与CMake做C++开发环境
date: 2022-07-11 21:37:50
tags: [CMake,VScode,C++,Windows]
categories: CMake,C++
index_img: https://blog-1253996024.cos.ap-beijing.myqcloud.com//v2-49a45b8c622434dbf79463ccab9fa627_720w.png
banner_img: https://blog-1253996024.cos.ap-beijing.myqcloud.com//v2-49a45b8c622434dbf79463ccab9fa627_720w.png
---

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//v2-49a45b8c622434dbf79463ccab9fa627_720w.png)



### 啥事cmake?

​		CMake 是一个开源、跨平台的工具，旨在构建、测试和打包您的程序。CMake 用于使用简单的平台和编译器独立配置文件来控制程序编译过程，并生成可在您选择的编译器环境中使用的主机配置文件和项目文件。这套 CMake 工具由 Kitware 创建，以满足 ITK 和 VTK 等开源项目对强大的跨平台构建环境的需求。

​		说白了，这玩意只是个配置器，配置你的编译器该如何进行编译

### cmake的好处：



	<center>“Write once, run everywhere”
	    
	</center>



​		非常好理解，就是只写一次，到处运行

​		对我来说就三个字：跨平台。

### cmake的缺点：

​		缺点：麻烦，步骤繁多		

​		断点调试挺麻烦的，VScode里的插件可以，但是需要对luanch.json和tasks.json进行修改，尤其是出现多个目录多个源文件等大项目的编译，修改虽然不复杂，但是麻烦。想要调试添加额外配置参数来打印所执行的所有代码及行号，插入log等方式检查问题。

​		有这弱语言的共通问题，就是容易出问题。

总而言之，挺麻烦的

~~要不是最近需要将Qmake项目迁移到cmake，还真不想用这玩意~~

### 利用CMake编译一个简单的C++程序

首先vscode需要下载两个插件，CMAKE与C/C++,这些我默认就有，因为在下载之前就一直用着VS2022，模块化无脑安装。

然后配置CMake和MINGW环境变量，这些我默认也有，安装Qt自带，直接指向Qt/tools里的对应路径即可。

前提条件准备得那是相当充足。

直接就随便顺手写一个冒泡排序的小例子

```
//创建这样一个目录结构
└── src
    	└── sort.cpp
└── include
    	└── sort.h
└── main.cpp    	
```



main.cpp

```c++
#include<iostream>
using namespace std;
#include"sort.h"

int main()
{
    int a[10]={3,1,2,4,5,6,8,9,10,7};
    Sort sort;
    sort(a);
    for (int i = 0; i < 10; i++)
    {
        /* code */
        cout<<a[i]<<endl;
    }
    sort.test();
    return 0;
}
```



sort.h

```c++
#pragma once
#include<iostream>
using namespace std;

class Sort
{
    public:
    int x[10];
    int temp;

    void test();


    void operator()(int x[10])
    {

        for(int i=0;i<10;i++)
        {   
            for (int j = 0; j < 10-i-1; j++)
            {
                 
                if(x[j]<x[j+1])
                {
                    temp=x[j];
                    x[j]=x[j+1];
                    x[j+1]=temp;
                }

            }

        }

    }

};


```

sort.cpp

```

#include"sort.h"
void Sort::test()
{
    cout<<"test\n";

}

```



到这里，前提源文件准备工作结束

开始编写CMakelists.txt

```cmake
#这种写法非常简单，无脑摁写，但是不够直观
project(Sorttest) #项目名称
aux_source_directory(src SRC_SUB) #项目根目录下的所有子项目
aux_source_directory(. SRC_CUR)  #同理
add_executable(sort ${SRC_SUB} ${SRC_CUR}) #同理
include_directories(include) #头文件包含目录
```



```cmake
cmake_minimum_required(VERSION 3.0) #最低版本需求
project(Sorttest) #项目名

include_directories(${PROJECT_SOURCE_DIR}/include)
#头文键包含

#添加源文件
add_executable(main
               ${PROJECT_SOURCE_DIR}/src/main.cpp #这个路径看这个main.cpp位于哪里了              
               ${PROJECT_SOURCE_DIR}/src/sort.cpp)
```

后者相对前者直观一些，效果都是一样的

在CMakelist.txt视窗中按下快捷键

```
shift+ctrl+p
```

唤起

![image-20220711223231514](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220711223231514.png)

选择CMake配置，一切自动化帮你构建build文件夹目录

再新终端下方 cd跳转到build目录中

输入以下指令

```powershell
cmake ..
```

运行如下工具 (这个是根据build的中的Makefile中的规则经行对源文件进行编译)

```
mingw32-make.exe 
```

可以看到运行后在Bulid目录下多出一个sort.exe

在终端中输入运行

```
.\sort.exe
```

既可以看到程序的输出结果：

![image-20220711224250666](https://blog-1253996024.cos.ap-beijing.myqcloud.com//image-20220711224250666.png)

至此编译到此结束