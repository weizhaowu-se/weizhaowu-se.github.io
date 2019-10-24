title: AIDL
date: 2016-09-26
tags:
- android
 
---

<font style="font-family:微软雅黑">
## Aidl:android interface definition language
aidl是安卓中一种跨进程通信的方式。
#### 服务端
* 创建一个service来监听客户端的连接请求
* 创建aidl文件，声明将要供客户端使用的接口以及方法
* 在service中实现接口方法。
#### 客户端
* 绑定服务端的service
* 将服务端返回的Binder对象转化成AIDL接口的类型
* 那么我们就能够在客户端中实现调用AIDL的方法了。（跨进程方法调用）
