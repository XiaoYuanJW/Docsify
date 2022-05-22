---
title: Docsify
date: 2022-05-22 17:18:55
tags: Others
---

# Docsify

## Docsify简介

Docsify是一个动态生成的文档网站的工具，Docsify不会生成静态的HTML文件，它会在运行时完成对Markdown文件的加载和解析。

本文将主要介绍如何使用Docsify快速搭建一个快捷、轻量级的个人&团队文档。

## 初始化项目

### 安装Node.js

Node.js是JavaScript运行环境，支持JavaScript运行在服务端的开发平台

Node.js官网地址：https://nodejs.org/

![202205201211462](https://s2.loli.net/2022/05/21/TOC8w2ZSvfdFgJN.png)

![202205201226136](https://s2.loli.net/2022/05/21/GIvEb4esqrDlFTy.png)

### 安装docsify-cli工具

docsify-cli进行本地初始化和实时预览

```
npm i docsify-cli -g
//推荐使用图内镜像
cnpm i docsify-cli -g
```

![image-20220522173914010](https://s2.loli.net/2022/05/22/3ty5M4Lz1g2VlKN.png)

### 初始化项目结构

新建一个docs文件夹，通过init初始化项目

```
docsify init ./docs
```

![image-20220522174002177](https://s2.loli.net/2022/05/22/HBorcbEiqQXhdS8.png)

![image-20220522174024315](https://s2.loli.net/2022/05/22/DPrEomUQW5nuMZt.png)

![image-20220522180143594](https://s2.loli.net/2022/05/22/GbMueCUpT1ZSrDs.png)

### 本地运行

```
docsify serve docs
```

![image-20220522174218091](https://s2.loli.net/2022/05/22/A24lQ35SWLxprsO.png)

![image-20220522174156828](https://s2.loli.net/2022/05/22/zgjxPEOnU1WFCBR.png)

### 基础配置文件

#### 基础配置项-index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Docsify By XiaoYuanJW</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport" content="width=device-width, user-scalable = no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <!-- 设置图标 -->
  <link rel="icon" href="/favicon.ico" type="image/x-icon" />
  <link rel="shortcut icon" href="/favicon.ico" type="image/x-icon" />
  <!-- 默认主题 -->
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
</head>
<body>
  <!-- 加载中 -->
  <div id="app">loading...</div>
  <script>
    window.$docsify = {
      name: 'Docsify By XiaoYuanJW',//项目名称
      repo: 'https://github.com/XiaoYuanJW',//仓库地址
      loadSidebar: true,//侧边框支持
      loadNavbar: true,//导航栏支持
      coverpage: true,//封面支持
      maxLevel: 5,//设置最大支持渲染的标题层级
      subMaxLevel: 4,//设置生成目录的最大层级
      mergeNavbar: true,//小屏设备合并导航栏到侧边栏
      alias: {
        '/.*/_sidebar.md': '/_sidebar.md'//防止意外回退
      },
      alias: {
        '/.*/_navbar.md': '/_navbar.md'//防止意外回退
      }
    }  
  </script>
  <!-- 搜索配置 -->
  <script>
    window.$docsify = {
      search: {
        maxAge: 86400000,//过期时间，单位毫秒，默认一天
        paths: auto,//注意：仅适用于paths: 'auto'模式
        // 支持本地化
        placeholder: {
          '/zh-cn/': '搜索',
          '/': 'Type to search'
        },
        noData: 'can not find results',
        depth: 4,
        hideOtherSidebarContent: false,
        namespace: 'Docsify By XiaoYuanJW',
      },
    }
  </script>
  <!-- Docsify v4 -->
  <script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
  <!-- docsify的js依赖 -->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
  <!-- emoji表情支持 -->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
  <!-- 图片放大缩小支持 -->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/zoom-image.min.js"></script>
  <!-- 搜索功能支持 -->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
  <!--Click to copy支持-->
  <script src="//cdn.jsdelivr.net/npm/docsify-copy-code/dist/docsify-copy-code.min.js"></script>
  <!-- 代码高亮 -->
  <script src="//unpkg.com/prismjs/components/prism-bash.js"></script>
  <script src="//unpkg.com/prismjs/components/prism-java.js"></script>
  <script src="//unpkg.com/prismjs/components/prism-sql.js"></script>
</body>
</html>
```

#### 封面配置文件-_coverpage.md

```markdown
  # Java学习记录
  > Notes of Java Created by XiaoYuanJW

  Java学习过程和记录

  [GitHub](https://github.com/XiaoYuanJW)
  [Get Started](README.md)
```

#### 侧边栏配置文件-_sidebar.md

```markdown
  * 基础篇
    * [MySQL](basic/MySQL.md)
  * 架构篇
    * [SpringBoot](architect/SpringBoot.md)
    * [MyBatis](architect/MyBatis.md)
    * [SpringCloud](architect/SpringCloud.md)
```

#### 导航栏配置文件-_navbar.md

```markdown
  * 网站
    * [myBlog](https://xiaoyuanjw.github.io/)
    * [Leetcode](https://leetcode.cn/u/xiaoyuanjw/)
  * 项目地址
    * [myMall](https://github.com/XiaoYuanJW/mymall_pro)
```

### 部署远端-Github

#### 新建代码仓库

![img](https://pic3.zhimg.com/80/v2-aa77dbcb386e9a433d0a37aaba306876_720w.png)

#### 提交仓库

```
git init
git remote add origin https://github.com/XiaoYuanJW/Docsify.git
git add *
git commit -m "first commit"
git push origin master
```

![image-20220522215447029](https://s2.loli.net/2022/05/22/tk1vo6yR4FnIT9V.png)

#### 开启GitHub Pages服务

![image-20220522224557192](https://s2.loli.net/2022/05/22/PGOi2RDIUQEo6A1.png)

![image-20220522224450431](https://s2.loli.net/2022/05/22/HYZlx8MC7qWXfjS.png)

![image-20220522225032813](https://s2.loli.net/2022/05/22/SyUNLaZ9TjEHWJV.png)