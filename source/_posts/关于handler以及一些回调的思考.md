---
title: 关于handler以及一些回调的思考
date: 2016-06-23 18:30:21
tags: android
---

 
---
<font style="font-family:微软雅黑">
有线程如下：

	public class SetTextThread extends Thread {
	    private IIsetText iIsetText;
	    private  Handler handler;
	    private  MyNum myNum;
	    public SetTextThread(IIsetText iIsetText) {
	        this.iIsetText = iIsetText;
	    }
	    public SetTextThread(Handler handler) {
	        this.handler = handler;
	    }
	    public SetTextThread(MyNum myNum) {
	        print("constructor:" + Thread.currentThread());
	        this.myNum = myNum;
	    }
	    @Override
	    public void run() {
	        print("run:" + Thread.currentThread());
	//        iIsetText.setText();
	//        handler.sendMessage(new Message());
	        myNum.i = 9999;
	    }
	
	    public void print(String s) {
	        System.out.println(s);
	    }
	}

* 多线程中，变量虽然相同，但是其实他们指向的是不同的内存区域，所以修改是不会影响到互相的值的。//此处有待确认正确与否，待查看thinking in java后再行确定。
* handler中，首先要先了解handler的消息机制
	
		MessageQueue：消息队列
		Looper：循环，一直从消息队列中获得消息，如果没有消息则一直循环。成员变量有MessageQueue。ThreadLocal变量，每个线程唯一且相同。
		Handler：成员变量有Looper，亦即也拥有MessageQueue，发送消息时向消息队列插入消息，有Looper负责发现消息并且根据Message的target（亦即handler）来负责处理消息。
所以，当我们在子线程中用handler发送消息时，虽然说此时的handler是另外线程中主线程handler的复制品，但是内容是完全一样的，亦即其Looper与消息队列依然是主线程的，所以，当发送消息时，插入的MessageQueue依旧是主线程，此时由handler的操作便变成了主线程的操作而非在子线程中操作界面。
