---
layout:     post
title:      Angular4+ionic3表单元素正则验证
subtitle:   正则验证
date:       2017-06-27
author:     Pangmr
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - ionic3
    - Angular4
    - 表单验证
---

项目开发的时候，经常会遇到表单元素的动态验证的需求，如下图所示。
![Alt ](/img/2017062701.png)
如果使用过Angular1.X版本开发过，那么Angular4自然也是相通的，只是语法不同。
现在主要说说在使用ng4的正则验证遇到的坑。

# Angular1.X正则验证
        ```
        <form name="userForm" class="form-horizontal" ng-submit="saveUserInfo(userForm.$valid)" novalidate>
            <div class="form-group" ng-class="{'has-error' :(userForm.userMobile.$invalid && userForm.userMobile.$touched)||(userForm.userMobile.$invalid && !userForm.userMobile.$error.required)}">
                <label class="col-sm-2 control-label"><span class="redTip">*</span>手机：</label>
                     <div class="col-sm-10">
                        <input type="text" class="form-control" name="userMobile"
                                           maxlength="11"
                                           ng-model="user.mobile"
                                           ng-pattern="/^1[34578]\d{9}$/"
                                           ng-class="{'text-danger':
                                           (userForm.userMobile.$invalid && userForm.userMobile.$touched)
                                            ||(userForm.userMobile.$invalid && !userForm.userMobile.$error.required)}" required>
                        <p class="text-danger" ng-if="userForm.userMobile.$error.required && userForm.userMobile.$touched">手机号码不能为空！</p>
                        <p class="text-danger" ng-if="userForm.userMobile.$invalid && userForm.userMobile.$dirty
                                                      || (userForm.userMobile.$invalid && !userForm.userMobile.$error.required)">手机号码格式错误,请输入11位数字！</p>
                     </div>
            </div>
        </form>
        ```

   
> 注意了这里的正则表达式的写法`ng-pattern="/^1[34578]\d{9}$/"`，这样是没问题的。

# Angular4正则验证（错误示范）
        ```
         <form [formGroup]="addressForm" novalidate>
            <ion-item [class.invalid]="!addressForm.controls.phone.valid  && addressForm.controls.phone.touched">
                 <ion-label fixed>联系电话</ion-label>
                 <ion-input type="text" maxlength="11" placeholder="请输入"
                  formControlName="phone" 
                  pattern='/^1[34578]\d{9}$/'
                  [(ngModel)]="address.phone" clearInput></ion-input>
            </ion-item>
            <ion-item *ngIf="!addressForm.controls.phone.valid  && addressForm.controls.phone.touched">
                 <p ion-text color="danger">请输入联系电话</p>
            </ion-item>
            <ion-item *ngIf="!addressForm.controls.phone.valid  && addressForm.controls.phone.dirty">
                 <p ion-text color="danger">联系电话格式错误</p>
            </ion-item>
         </form>
        ```

> 那么问题就来了，在这里怎么输入正确的电话号码都不通过(==!!!)


然后就在网上查了很多资料，最终通过github上Angular的Issues圆满解决了问题。
来看看解决的过程：
1. 通过Issues得知ng2+在获取pattern上的正则时，会自动加上首尾的`^`和`$`(黑人问号脸：what？)。
为了验证这个问题，可以在后台ts文件中打印出这个control，看看究竟是怎么回事。
![Alt ](/img/2017062702.png)
果然，ng2+会自动加上首尾的`^`和`$`（这不坑爹吗，延续版本1的写法不好吗。。。）
OK，现在去掉首尾的`^`和`$`，再来一遍。

![Alt ](/img/2017062703.png)
事实证明，还是不行。。。（有点小崩溃了）
找BUG的过程省略。。。
2. 最终定睛一看，这个正则有点不对劲啊！原本的正则是`^1[34578]\d{9}$`,而打印出来的正则是`^1[34578]d{9}$`。
原来少了个\，也就说ng2+正则还需要转义！！！
所以正则的正确写法是：`^1[34578]\\d{9}$`
![Alt ](/img/2017062704.png)

# Angular4正则验证（正确示范）
        ```
         <form [formGroup]="addressForm" novalidate>
            <ion-item [class.invalid]="!addressForm.controls.phone.valid  && addressForm.controls.phone.touched">
                 <ion-label fixed>联系电话</ion-label>
                 <ion-input type="text" maxlength="11" placeholder="请输入"
                  formControlName="phone" 
                  pattern='1[34578]\\d{9}'
                  [(ngModel)]="address.phone" clearInput></ion-input>
            </ion-item>
            <ion-item *ngIf="!addressForm.controls.phone.valid  && addressForm.controls.phone.touched">
                 <p ion-text color="danger">请输入联系电话</p>
            </ion-item>
            <ion-item *ngIf="!addressForm.controls.phone.valid  && addressForm.controls.phone.dirty">
                 <p ion-text color="danger">联系电话格式错误</p>
            </ion-item>
         </form>
        ```
        
        
最后总结ng2+使用正则表单验证的时候切记不要直接把正则粘贴到pattern，记得去掉首尾的`^`和`$`，还有反斜杠要转义！！！








    
































