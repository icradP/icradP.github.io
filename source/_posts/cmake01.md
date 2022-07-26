---
title: CMake基础
tags: [CMake,vscode]
date: 2022/7/12
categories: CMake
comments: false
---



## [Cmake]() 



### 前言

Linux下cmake编译工具使用过程与windows的操作大同小异

单独开一篇文章对指令基础做个总结



### Corss-platform development

跨平台：毕竟做linux下的c++ 开发不用cmake 可不行



### 语法特性

指令格式

```
指令（参数1 参数2）
```

不同参数需要用**空格**或者**分号**隔开

指令无关大小写，**参数**和**变量**是大小写**相关**的



#### 常用指令

- **cmake_minimum_required**:指定cmake版本

  ```cmake
  #cmake_minimum_required -指定cmake的最小版本要求
  cmake_minimum_required (VERSION 2.8.3)
  
  ```

- **project**:定义工程名称

  ```cmake
  #指定 project(projectname [CXX][C][java])
  project(HEllO CXX)
  #指定工程名称为HELLO 支持C++ 
  ```

- **set**:显示的定义变量

  ```cmake
  set(SRC sayhello.cpp hello.cpp)
  #定义一个SRC变量 引用SRC 相当于引用sayhello.cpp hello.cpp
  ```

- **include_directories **向工程添加多个特定的头文件搜索路径

  假设文件夹路径是这样的格式

  ```
  └── myfolderinclude
      └── othersfolderinclude
  ```

  ```cmake
  include_directories(../myfolderinclude ./othersfolderinclude)
  #类似于g++中的-I 
  ```

- **link_directories**:向工程添加多个指定的库文件搜索路径

  ```cmake
  link_directories(../myfolderlib ./othersfolderlib)
  ```

- **add_library**:生成库文件

  比较重要的语法

  ```cmake
  # add-library(libname[SHARED|STATIC|MODULE][EXCLUDE_FORM_ALL]source1 source2 ..sourceN)
  #通过变量SRC （上方代码中定义的SRC变量引用）生成动态[SHARED] 的libhellp.so文件
  add_library(hello SHARED ${SRC})
  #注意，变量的引用使用${}
  
  ```

- **add_compile_options**：添加编译参数

  ```cmake 
  add_compile_options(-Wall -std=c++ -O2)
  #分别添加的是警告显示，定义语言特性，优化级数
  ```

- **add_executable** :生成可执行文件

  ```cmake
  add_executable(main main.cpp)
  #将main.cpp编译为 mian
  ```

- **target_link_libraries**为target链接的动态库

  ```cmake
  #等价于g++ -l 编译器参数
  target_link_libraries(main hello)
  #将hello动态库链接到可执行文件main 
  ```

- **add_subdirectory**向当前的工程添加存放源文件的子目录，指定中间二进制和目标二进制的存放位置

  ```cmake
  add_subdirectory(src)
  #添加一个src子目录,src中需要也有一个CMakelists.txt
  ```

- **aux_source_directory** 发现一个目录下所有的源代码文件并将列表存储在一个变量中，这个指令临时被用来自动构建源文件列表

  ```cmake
  aux_source_directory(. SRC)
  #定义. （就是一个目录下所有的源文件） 为SRC变量
  add_exrcutable(mian ${SRC})
  #引用SRC 生成可执行文件
  ```

  

#### 常用变量

- **CMAKE_CXX_FLAGS**：编译选项

  ```cmake
  #在CMAKE_CXX_FALGS编译选项后加 -std=c++11
  set(CMAKE_CXX_FALGS "${CMAKE_CXX_FLAGS} -std=c++11")
  #类似CMAKE_CXX_FALGS+=-std=c++11
  ```

- **CMAKE_BUILDTYPE**：编译类型

  ```cmake
  set(CMAKE_BUILD_TYPE Debug)
  #编译为Debug
  set(CMAKE_BUILD_TYPE Release)
  #编译为Release
  ```



### CMake编译

手动编写CMakeLists.txt

#### 两种编译规则

- ​	子目录无CMakeLists.txt

- ​	子目录包含CMakeLists.txt

#### 两种构建方式

- 内部构建（不用

  ```
  单纯的直接cmake 当前文件夹生成makefile
  再make 执行makefile 生成可执行文件等等，
  因为都生成在当前文件夹很乱，目录结构不清析。
  所以不用
  ```

  

- 外部构建（常用

  ```bash
  #在当前目录下新建一个build 
  mkdir build 
  #跳转
  cd build 
  #执行cmake,执行上层主目录下的CMakelists.txt
  cmake .. 
  #生成makefile和其他文件
  make 
  #执行
  #所有的编译输出文件都存在了build目录中
  #互不干扰结构明了
  ```

  

### CMake在vs.code下的甜点

​		当我们需要在vs.code调试CMake代码生成的可执行文件的时候，可以设置launch.json和一个task.json的调试脚本~~个人觉得，只是锦上添花的操作，除了tasks.josn,我们可以写bat，或者shell都可以而且代码更少~~，达到可以直接运行调试的效果，不需要在修改源代码后，再手动输入cmake与make 重新编译，实现自动化操作

​		首先我们选择运行与调试默认生成的launch.json，给出一个文件目录的示例

```bash
.
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles/....
│   ├── cmake_install.cmake
│   ├── Makefile
│   └── main#这个是生成的可执行文件
├── CMakeLists.txt
├── include
│   ├── gun.h
│   └── soilder.h
├── main.cpp
└── src
    ├── gun.cpp
    └── soilder.cpp
```

根据文件目录我们将默认生成的launch.json进行改动

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) 启动",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/main",//这里需要更改为你的可执行文件
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "将反汇编风格设置为 Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ],
            
            "preLaunchTask": "Build", //这是里我们需要修改的地方
            //上方代码的意思是，执行调试前的前置任务，所以我们需要再生成一个tasks.json文件
            "miDebuggerPath": "/usr/bin/gdb"//默认添加
            
        }
    ]
}
```

再编写一个简单的tasks.json，可以直接在vs.code终端菜单下直接生成一个简单的任务范例

```json
{
    "version": "2.0.0",
    "options": {
        "cwd": "${workspaceFolder}/build"//指定工作区下的目录
    },
    "tasks": [
        {
            "label": "step1",
            "type": "shell",
            "command": "cmake",
            "args": [
                ".."  //参数
            ]
        },
        {
            "label": "step2",
            "group": {
                "kind": "build",
                "isDefault":true
            },
            "command": "make",
            "args": [
                
            ]
        },
        {
            "label": "Build",
            "dependsOrder": "sequence",//依赖顺序设置为顺序执行
            "dependsOn":[   //顺序
                "step1",
                "step2"
            ]
        }
    ]
}
```

