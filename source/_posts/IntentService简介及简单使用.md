---
title: IntentService简介及简单使用
date: 2016-10-07 13:58:04
tags:
- android

---
<font style="font-family:微软雅黑">
## 使用
1. 创建类继承IntentService  

		public class CustomIntentService extends IntentService {
		    /**
		     * Creates an IntentService.  Invoked by your subclass's constructor.
		     *
		     */
		    public CustomIntentService() {
		        super("custom intent service");
		    }
		
		    @Override
		    protected void onHandleIntent(Intent intent) {
				LogUti.d("on handle intent");
		        LogUti.d(intent.getStringExtra("key"));
		        SystemClock.sleep(6000);
		        LogUti.d("after 6000ms");
		    }
		}
2. 在manifest中定义service

		<service android:name="custome_service.CustomIntentService"></service>

3. 在activity中启动IntentService
		
		findViewById(R.id.btn_start_intent_service).setOnClickListener(new View.OnClickListener() {
		            @Override
		            public void onClick(View v) {
		                Intent intent = new Intent(IntentServiceActivity.this, CustomIntentService.class);
		                intent.putExtra("key", "value");
		                startService(intent);
		            }
_启动服务之后调用的是onHandleIntent方法_  
_注意onHandleIntent方法不运行在UI线程所以没有办法进行界面的操作：例如toast显示_  
_结果如下图_  
![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-7/78666379.jpg)

4. IntentService与Activity进行交互  
_通过本地广播的方式进行_

 * 在IntentService中
 
	         Intent localIntent = new Intent(CustomConstans.BROADCAST_ACTION);
	        localIntent.putExtra(CustomConstans.BROADCAST_KEY, "from intent service");
		     LocalBroadcastManager.getInstance(this).sendBroadcast(localIntent);
 * 在Activity中
	 * 新建类继承broadcastreceiver
		 
			    private class ResponseReceive extends BroadcastReceiver{
			
			        @Override
			        public void onReceive(Context context, Intent intent) {
			            Toast.makeText(context, intent.getStringExtra(CustomConstans.BROADCAST_KEY)
			            , Toast.LENGTH_SHORT).show();
			        }
			    }
		* 注册广播
		
		        IntentFilter intentFilter = new IntentFilter(CustomConstans.BROADCAST_ACTION);
		        ResponseReceive responseReceive = new ResponseReceive();
		        LocalBroadcastManager.getInstance(this).registerReceiver(responseReceive, intentFilter);
## tips
* onHandleIntent方法运行在后台线程，无法进行界面操作。
* 通过本地广播的方式来跟activity进行交互。
* Work requests run sequentially. If an operation is running in an IntentService, and you send it another request, the request waits until the first operation is finished.顺序执行