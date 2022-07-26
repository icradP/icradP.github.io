---
title: 博客的更新与备份
tags: [Blog,hexo]
date: 2022/7/14
categories: 博客技巧
comments: false
---

## 博客的更新与备份

基于hexo的博客，搭建在github的仓库中

我们要实现在不同的系统与电脑中进行修改，与同步

### 需要以下条件

1. Nodes.js
2. npm
3. git
4. 博客源文件已经部署在github的仓库中（建议部署在分支中）

### 需要注意的事项

首先新系统下的git配置，关键点在于重新配置ssh密匙

其次是Nodes.js与npm需要最新版本（有多新装多新



### 初始化另一台电脑的操作

1. git bash 将远程仓库克隆到本地

   ```bash
   git clone 博客所在仓库地址
   ```

2. 进入项目目录，安装依赖启动博客服务器，生成静态文件

   并在本地部署，通过http://localhost:4000进行访问

   ```bash
   npm install
   hexo g&hexo s
   ```

3. 发布文章与之前相同

   ```bash
   hexo c&hexo d -g
   ```

   

### 另一台电脑同步

​		在博客目录下执行

```
hexo clean
hexo d -g
```

​		后执行更新原始文件

```bash
git pull 
git add -A
git commit -m "描述"
git push origin hexo 
```

​		每次有新的操作的时候，在另一台电脑上也同时进行更新

```bash
git pull hexo #拉取源文件
```