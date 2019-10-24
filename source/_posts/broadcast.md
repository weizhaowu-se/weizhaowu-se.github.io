title: 广播接收器
date: 2016-09-26
tag:
- android
- 第一行代码

---
<font style="font-family:微软雅黑">
## 广播机制
* 标准广播
	> * 完全异步执行
	> * 所有广播接收器几乎同时收到
* 有序广播
	> * 同步执行
	> * 优先级高的先收到，可以截断。
## 系统广播

## broadcast receiver
* 不允许开启线程
* onReceive 中的context的来源，是谁在调用这个函数 // todo
### 静态注册
>  manifest中注册
  
	<receiver android:name="util.NetworkChangedReceiver">
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE"></action>
            </intent-filter>
        </receiver>
### 动态注册
> 代码中注册

1. 继承类

		    class NetworkChangeReceiver extends BroadcastReceiver{
	
	        @Override
	        public void onReceive(Context context, Intent intent) {
	            ConnectivityManager manager = (ConnectivityManager) getSystemService(CONNECTIVITY_SERVICE);
	            NetworkInfo info = manager.getActiveNetworkInfo();
		/*
	            Intent intent1 = new Intent(context, SettingActivity.class);
	            startActivity(intent1);
		*/
	            if (info != null && info.isAvailable()) {
	                Toast.makeText(context, "network is available", Toast.LENGTH_SHORT).show();
	            } else {
	                Toast.makeText(context, "network is unavailable", Toast.LENGTH_SHORT).show();
	            }
	        }
	    }
 
2.  注册广播，在onCreate中添加

	        IntentFilter intentFilter = new IntentFilter("android.net.conn.CONNECTIVITY_CHANGE");
	        registerReceiver(receiver, intentFilter);

3. 注意要在onDestroy函数中取消注册。

## 自定义广播接收器
* 与隐式intent类似，可以在intent-filter中添加自定义的action，发送广播时调用sendBroadCast(Intent intent)。此时发送的是标准广播，同时也可以发送一些数据，在intent当中。
* sendOrderedBroadCast() 发送有序广播
### 本地广播功能
LocalBroadCastManager getInstance()方法
  
* 初始化localBroadCastManager
>localBroadCastManager = LocalBroadCastManager.getInstance(this);

* 注册
>localBroadCastManager.register.....

* 发送
> localBroadCastManager.send......

* 销毁
> localBroadCastManager .unregister.....

* 本地广播无法使用静态注册。 
## tips
* 在广播接收器里启动activity时，需要给Intent加入FLAG\_ACTIVITY\_NEW\_TASK标志。