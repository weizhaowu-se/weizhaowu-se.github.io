title: Android笔试题目
date: 2016-09-26
tags:
- android
 
---
<font style="font-family: 微软雅黑;">
title: Android基础笔试题
tags:
- android
- 笔试
# activity的启动过程
> onCreate()->onStart()->onResume()->运行->onPause()->onStop()->onDestroy.

* 注意，当启动新的activity或切换到桌面时，回调onPause和onStop，切换回来的时候，回调onRestart->onStart->onResume。
* back回退时，回调onPause，onStop，onDestroy。
* 如果被系统回收的话，则与初次启动回调函数一样，但是在onCreate中会有个状态保存的变量供恢复数据
* 旧Activity的onPause先调用，然后新Activity才启动。
* 意外情况activity被销毁的时候，会调用onSaveInstanceState保存状态，再将Bundle对象传递给onRestoreInstanceState和onCreate方法。
* 资源内存不足导致activity被杀死
* 优先级：前台>可见但非前台（弹出对话框的activity）>后台（已经被暂停的）

# activity的启动模式有
* 标准模式
* singleTop 栈顶复用模式，如果在栈顶，则不重新创建，同时调用onNewIntent方法
* singleTask  栈内复用,在一个新的task中产生这个实例，以后每次调用都会使用这个。
* singleInstance 单实例模式，与singleTask模式基本一样，区别在于在这个模式下的activity实例所处的task中只有这个实例而不会有其他的实例。

# 数据存储方式
* sharedpreferrence：xml格式，简单的键值对，有缓存，注意多线程访问时候可能出现的问题
* 普通文件存储
* SQLite数据库：轻量级的数据库，使用openHelper，cursor，SQLiteDataBase。
* ContentProvider。cursor，provider。
* 网络。httpURLconnection。



