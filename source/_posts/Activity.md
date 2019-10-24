title: Activity
date: 2016-09-26
tag:
- android
- 第一行代码

---
<font style="font-family:微软雅黑">
## Activity
### res目录
* drawable 存放图片
> * R.drawable.XXX
> * @drawable/XXX

* values 存放字符串
> * 代码中：R.string.XXX	
> * xml中：@string/XXX


* layout 存放布局文件
* menu 存放菜单文件

* 项目中任何添加的资源都会在R文件中生成一个相应的资源id。

### 添加菜单
1. res下新建menu文件夹，添加菜单文件（res的menu文件夹中）
2. 重写onCreateOptionsMenu方法

	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
	        this.getMenuInflater().inflate(R.menu.main, menu);
	        return true;
	    }
3. 响应点击按钮事件

	    @Override
	    public boolean onOptionsItemSelected(MenuItem item) {
	        switch (item.getItemId()) {
	            case R.id.set_item:
	                Toast.makeText(this, "设置", Toast.LENGTH_SHORT).show();
	        }
	        return true;
	    }

### Intent
#### 隐式intent
##### 待启动的activity
* 在需要响应内容的Activity中添加action以及category字段，只有当这两个字段完全匹配上intent的相应的值时，才可以启动activity。
* intent构造函数：action字符串传入，默认情况下会插入

	>"android.intent.category.DEFAULT"

* intent-filter可以指定data标签，更精确的指定当前activity能够响应的数据

		<data android:scheme="http"
            android:mimeType="XXX"></data>
除此之外，还能够匹配host，port，path等，只有当这些值完全匹配，才会被启动。
##### 隐式启动activity时
* intent 可以指定多个category
* 


                    Intent intent  = new Intent(Intent.ACTION_VIEW);
                    intent.setData(Uri.parse("http://www.baidu.com"));


                    Intent intent = new Intent(Intent.ACTION_DIAL);
                    intent.setData(Uri.parse("tel:10086"));
* intent setDateAndType（，）两个参数分别是uri以及文件类型.
[http://blog.csdn.net/chaoyu168/article/details/50778016](http://blog.csdn.net/chaoyu168/article/details/50778016)
##### activity之间传递数据
* 传递给下一个界面
	> * 在intent当中putExtra就可以了,在下一个界面当中getIntent获得intent就可以读取里面的内容了。
	  
	> * 可以直接使用key-value的方式插入intent中，也可以通过bundle类来进行插入，bundle类类似一个map，能够简化操作。
	> * 实现了序列化接口的类对象也可以通过intent传递。
* 对接时怎么样传递数据比较好呢？
	> 在被启动的页面中新增一个静态方法actionStart(),参数需要有context，然后参数列表是需要传递的数据，这样在前一个页面中直接调用这个方法就可以启动新的页面了。

* startActivityForResult
	> * 启动时传入requestCode
	> * 重写onActivityResult方法，根据参数中的requestCode判断是哪个的回调。
	> * 必要时还需要重写被启动页面的返回键回调函数onBackPress()

##### 生命周期
![](http://7xkzud.com1.z0.glb.clouddn.com/16-9-23/53552719.jpg)

##### Activity被回收了
* onSaveInstanceState(Bundle)
* onCreate(Bundle)

##### 启动模式
* standard模式
* singleTop模式
	> * 栈顶如果是要启动的activity，则不新建，仅此而已。

* singleTask模式
	> * 检查返回栈中是否存在类的实例，如果存在将其上的所有activity出栈。
* singleInstance模式
	> * 存放在一个新的返回栈里面。

