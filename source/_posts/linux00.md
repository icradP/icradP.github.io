---
title: linux基础00
tags: [linux]
date: 2022/7/11
categories: linux
comments: false
---



## linux基础中的基础

### linux  的基础命令

#### 指令默认格式

```bash
指令 [选项] [操作对象]
```

注意：中括号代表的意思是可以省略内容不输入，一样可以运行该指令

默认打开终端

```
Ctrl+Alt+T
```



#### 常用指令

打印当前所在的目录

```bash
pwd
```

打印当前目录下的文件与文件夹

```bash
ls
```

ls的用法扩展：

```bash
ls [文件路径]
#文件路径：
#相对路径or绝对路径
```

打开文件目录下的文件

```bash
ls 选项 [文件路径]
#选项：指定方式显示
```

选项：

```bash
-a #表示显示所有的(all)文件与文件夹，包括隐藏
-l #表示详细列表的方式展示(list)
-h #以可读性较高的形式显示
```



- #### cd(chanage directory)

：跳转目录

```bash
cd  
cd ~
#作用都是直接回到家目录
```

与ls类似的

```bash
cd [文件路径]
#文件路径：
#相对路径or绝对路径
```



- #### mkdir(make directories)

```bash
mkdir myfolder 
#在当前的目录下创建一个mydir文件夹
mkdir myfolder1 myfolder2
#在当前目录下创建两个文件夹，分别是 myfolder1，myfolder2
```

```bash
mkdir -p ~/a/b/c
#-p 选项是必须的
#当前家目录下(~)创建多层不存在的目录
```

- #### touch-change file timestamps

作用：创建文件（虽然本意不是这个）

```bash
touch  test.txt
#在当前目录下生成一个 test.txt文件
touch ../test1  #（相对路径)
#在当前目录的上级目录下生成一个 test1文件
touch ~/test2  #(绝对路径)
#在~（家)目录下生成一个 test2文件
```

- #### rm (remove )

删除指令

```bash
rm [选项] [路径]
```

```bash
rm test.txt
#删除文件
rm ../test1
#删除上级目录的文件
rm ~/test2
#在~（家)目录下删除test2文件
```

删除文件夹和文件夹下的文件

```bash
rm -rf [文件夹路径]
#删除文件夹
```

- #### cp(copy)

作用：复制文件

```bash
cp [复制的文件路径] [副本的文件路径]
cp ../test ./test.txt
#上级的目录下的test文件复制到当前目录下 并且换了个名字（加了个.txt后缀名）
```

复制文件夹

```bash
cp -r [文件夹路径] [副本文件夹的路径]
#-r是递归的意思
```



- #### mv (move)

作用：移动文件位置，或者重命名文件

```bash
mv test test.txt
#重新命名test文件为 test.txt(在当前目录下)
#等效 mv test ./test.txt

mv test ../test.txt 
#重新命名并且移动到了上级目录中

```



- #### men (menu)

  说明书目录:

```bash
man ls
#参看命令ls的手册

#在man中无cd的手册
#我们在cd help中可以看到

man man 
#参看man的命令手册
```



- #### reboot

  重启

  ```bash
  reboot
  #立即重启
  ```

  

- #### shutdown

  关机

  ```bash
  shutdown -h [时间]
  
  shutdown -h now 
  #立即关机
  ```







### 文件的编辑

- ### vim

  一句话：**编辑器之神**

  安装：(Ubuntu LTS 22.04)

  ~~想用的时候居然没有安装~~

  ```bash
  sudo apt install vim
  
  #建议安装：
   ctags vim-doc vim-scripts
  #顺带一并安装#都是vim的插件
  sudo apt-get install ctags
  sudo apt-get install vim-doc
  sudo apt-get install vim-scripts
  
  ```

  ![助记图](https://img-blog.csdn.net/2018101511335895?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9jeTY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

  

  这里不继续详细的说明，将会新开一篇文章来记录VIM 的使用

  

  简单的使用：

  ```
  vim [file]
  #直接打开文本
  ```

  终端直接变成了文本编辑器

  键盘摁下 i 键

  可以可以看见左下角状态变更为 插入 （insert）

  此时即可以输入文本进行编辑了

  保存需摁下

  ```
  shift 
  ```

  输入： 

  ```
  :w #保存文本
  :q #退出vim编辑器
  ```

  

- #### geidt

  纯文本的编辑器，ubuntu 都是自带的,默认的编辑器

  

  简单的使用：

  ```bash
  vim [file]
  #直接打开文本编辑框，
  ```

  与vim不同点在于 弹出的geidt的编辑界面，对新手挺友好的

  直接按照Windows下的记事本使用即可

  

- #### nano

  与Vim类似，但是比vim简单而且小巧

  简单的使用:

  ```bash
  nano [file]
  #终端变成nano界面
  ```

  指令都在下方直观的显示出来

  修饰键是Ctrl ，使用起来非常简单