---
layout:     post
title:      Angular4+ionic3开发总结
subtitle:   webApp开发
date:       2017-06-05
author:     Pangmr
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - web
    - 前端开发
---














>最近公司开发的一个webapp项目，使用到了Angular4+ionic3技术，其实很早就想学习一下Angular2+的，
    刚好这次项目技术选型的时候用了最新版本。
>不过由于ng2+与ng1的语法概念大不相同，所以相当于重新学习一门框架，好在之前使用过Vue框架，对于组件化有些了解，不过也不可避免的踩了一些坑。
>所以今天就目前所知的理解，对Angular4+ionic3开发做一些自己的总结。

##准备工作
>在开发之前，需要准备的技术支持：
1. TypeScript2
2. ECMAScript6.0
>项目环境：
1. node
2. npm
>开发工具：
1. VSCode（typescript是由微软推出的，所以对typescript支持很好）

##新建项目
>使用ionic3框架搭建项目，因为该框架已经集成了Angular4.0。
1. 安装ionic和依赖cordova
`npm install -g ionic cordova`

2. 创建应用项目，因为只是webAPP，无需再安装其他依赖
`ionic start myApp [template name]`
这里有3种创建方式：blank（空白项目）、sidemenu（带侧边栏菜单）、tabs（带底部tab菜单栏）

3. 删除不需要的文件，目录结构如下：
![Alt 目录结构](/img/ionic-doc.png)





















