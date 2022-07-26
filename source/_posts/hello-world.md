---
title: 文章的技巧
tags: [Typora,md]
date: 2022/6/25
categories: 博客技巧
comments: false
---
###   前言：

​		第一次利用Typora编写Md文件并且发布博客，有很多操作还是非常生疏，在这里记录个Md文件的文章编辑的常用代码。以供我啥时候突然给整忘了回来看看😁

​									<!-- more -->

`                      

### 文章阅读截止

​		将过长的文章截取，只显示代码上方的文章内容，避免文章过长的显示在博客主页（这个可以说是非常常用）

代码如下：

``` bash
<!-- more -->
```

### 文章的新建命令

```
hexo new [layout] <title>
```

​		在github bash 中使用 该指令可以新建一个页面：

​		使用实例如下 ：

```
hexo new post 标题
```

​		可以在hexo根文件的中的_post的文件夹中发现新建了一个标题.md	，头部信息如下

```
title: 标题
date: //时间
tags: //标签
categories:  //分类
```

### 草稿的新建命令

```
hexo new draft title
```

​		在github bash 中使用 该指令可以新建一个草稿：

​		使用实例如下 ：

```
hexo new draft 标题
```

​		可以在hexo根文件的中的_drafts的文件夹中发现新建了一个标题.md	头部的信息如下

```
title: 标题
date: //时间
tags: //标签
categories:  //分类
```

​		草稿不会显示在博客中，想要看到博客草稿需要在github bash 中使用如下指令：

```
hexo s -draft
```


​		该指令的作用除了在本地运行博客部署以外，可以在博客中访问草稿.

### front  Matter

![](https://blog-1253996024.cos.ap-beijing.myqcloud.com//20200916181236318.png)

​	每个创建的MD文件都在头端插入了名为front  Matter预定义读取参数,就是上文所说的头部信息。
​	可以看见这张图是有水印的，我们可以利用MD的文件中的画图代码画出相同的信息：

<center>

|    参数    |       描述       |       默认值       |
| :--------: | :--------------: | :----------------: |
|   layout   |       布局       |         无         |
|   title    |       标题       |         无         |
|    date    |    建立的日期    |    文件建立日期    |
|   update   |    更新的日期    | 每次文件更新的日期 |
|  comments  | 该文章的评论功能 |  默认每个文章开启  |
|    tags    |       标签       |         无         |
| categories |       分类       |         无         |

</center>
