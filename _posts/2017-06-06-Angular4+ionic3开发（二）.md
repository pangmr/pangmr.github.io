---
layout:     post
title:      Angular4+ionic3开发（二）
subtitle:   webApp开发
date:       2017-06-06
author:     Pangmr
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - webApp
    - Angular4
    - ionic3
    - 前端开发
---

第一篇介绍了怎么创建项目，下来就来说说开发当中花费了很多时间解决的一些问题。

# ionic
1.页面跳转传参：常见的应用场景就是从列表页带ID，进入详情页。
    `this.navCtrl.push(DetailPage,{ id: id });`

详情页接收参数：
    `this.navParams.get('id')`
> 一般ng1的做法是在路由注册的时候传入，然后使用ng4的时候发现，好像根本没有路由这个东西了...

2.隐藏子页面的tab菜单。
    `IonicModule.forRoot(MyApp,{ tabsHideOnSubPages: 'true' })`

3. 弹出层选择数据后调用父页面的方法，弹出层选择的是Popover。
    、this.popoverCtrl.create(PopoverPage,{
            cb: function(_data){//回调函数
              //刷新页面，后续代码
            },
            param: param //传递给Popover的参数
          },{});、

Popover页面的操作：
    `callback: any;//定义回调函数
    this.callback = this.navParams.get('cb'); // 获取父页面的回调`
    然后就可以在Popover页面使用`this.callback()`，刷新父页面的数据了































