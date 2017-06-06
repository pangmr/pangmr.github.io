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

第一篇介绍了怎么创建项目，接下来就来说说开发当中花费了很多时间解决的一些问题。

# ionic

1. 页面跳转传参：常见的应用场景就是从列表页带ID，进入详情页。<br/>
        ```
        this.navCtrl.push(DetailPage,{ id: id });
        ```

   详情页接收参数：<br/>
        ```
        this.navParams.get('id')
        ```
> 一般ng1的做法是在路由注册的时候传入，然后使用ng4的时候发现，好像根本没有路由这个东西了...

2. 隐藏子页面的tab菜单。<br/>
        ```
        IonicModule.forRoot(MyApp,{ tabsHideOnSubPages: 'true' })
        ```

3. 弹出层选择数据后调用父页面的方法，弹出层选择的是Popover。<br/>

        ```
        ParentPage:

            this.popoverCtrl.create(PopoverPage,{
                cb: function(_data){//回调函数
                  //刷新页面，后续代码
                },
                param: param //传递给Popover的参数
              },
            {});
        ```
        ```
        ChildPopover：

            callback: any;//定义回调函数
            this.callback = this.navParams.get('cb'); // 获取父页面的回调
        ```
然后就可以在Popover页面使用`this.callback()`，刷新父页面的数据了。

ionic的疑难点先整理这些，以后遇到了再补充

# Angular
1. http请求
    > 开发的时候，找了半天也没找ng4的http服务，最后才查到，原来用的是RxJS。
    > RxJS: Rx是一个编程模型，目标是提供一致的编程接口，帮助开发者更方便的处理异步数据流
    > 传送门[ReactiveX]: http://reactivex.io/

    ```
    import { Injectable } from '@angular/core';
    import { Http, Response, Headers, RequestOptions, URLSearchParams, RequestOptionsArgs } from '@angular/http';
    import 'rxjs/add/operator/map';
    import 'rxjs/add/operator/toPromise';
    import {Observable} from "rxjs";

    @Injectable()
    export class HttpService{
      constructor(public http: Http){};

      public get(url: string, paramMap?: any): Observable<Response> {
        return this.http.get(url, new RequestOptions({
          search: HttpService.buildURLSearchParams(paramMap),
          headers: new Headers({
            'companyid': 1
          })
        }));
      }


      public post(url: string, body: any = null, options?: RequestOptionsArgs): Observable<Response> {
        return this.http.post(url, body, this.getOptions(options));
      }

      public postFormData(url: string, paramMap?: any): Observable<Response> {
        let headers = new Headers({
          'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
          'Accept': 'application/json;charset=utf-8'
        });
        return this.http.post(url, HttpService.buildURLSearchParams(paramMap).toString(), new RequestOptions({headers: headers}));
      }

      public put(url: string, body: any = null, options?: RequestOptionsArgs): Observable<Response> {
        return this.http.put(url, body, this.getOptions(options));
      }

      public delete(url: string, paramMap?: any): Observable<Response> {
        return this.http.delete(DEV.url + url, new RequestOptions({
          search: HttpService.buildURLSearchParams(paramMap)
        }));
      }

      public patch(url: string, body: any = null, options?: RequestOptionsArgs): Observable<Response> {
        return this.http.patch(url, body, this.getOptions(options));
      }

      public head(url: string, paramMap?: any): Observable<Response> {
        return this.http.head(url, new RequestOptions({
          search: HttpService.buildURLSearchParams(paramMap)
        }));
      }

      public options(url: string, paramMap?: any): Observable<Response> {
        return this.http.options(url, new RequestOptions({
          search: HttpService.buildURLSearchParams(paramMap),
          headers: new Headers({
            'companyid': 1
          })
        }));
      }

      public static buildURLSearchParams(paramMap): URLSearchParams {
        let params = new URLSearchParams();
        if (!paramMap) {
          return params;
        }
        for (let key in paramMap) {
          let val = paramMap[key];
          params.set(key, val);
        }
        return params;
      }

      private getOptions(options): RequestOptionsArgs {
        if (!options) {
          options = new RequestOptions({
            headers: new Headers({
              'companyid': 1
            })
          });
          return options;
        }
      }
    }


    ```

































