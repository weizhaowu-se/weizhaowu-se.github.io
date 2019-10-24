---
title: android序列化
date: 2016-10-08 15:30:31
tags:
- android
- java

---
<font style="font-family:微软雅黑">
## 实现Serializable接口
_Serializable接口是来自Java的序列化接口_
### 使用方法

	    class BagOfPrimitives implements Serializable{
	        public int value1 = 1;
	        public String value2 = "abc";
	        public transient int value3 = 3;
    	}
* 在Intent中传递对象时  
![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-8/9889133.jpg)
* 取出对象  
![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-8/4751825.jpg)
## 实现Parcelable接口
_Android自带的接口，性能更高，但通用性不强_
### 使用方法如下
* Parcel为参数的构造函数中，给变量赋值
* writeToParcel(Parcel dest, int flags)将变量的值写入dest

			public class Person  implements Parcelable {
			    private int data;
			    private Book book;
			    protected Person(Parcel in) {
			        data = in.readInt();
			        book = in.readParcelable(Book.class.getClassLoader());
			    }
			
			    public static final Creator<Person> CREATOR = new Creator<Person>() {
			        @Override
			        public Person createFromParcel(Parcel in) {
			            return new Person(in);
			        }
			
			        @Override
			        public Person[] newArray(int size) {
			            return new Person[size];
			        }
			    };
			
			    @Override
			    public int describeContents() {
			        return 0;
			    }
			
			    @Override
			    public void writeToParcel(Parcel dest, int flags) {
			        dest.writeInt(data);
			        dest.writeParcelable(book, flags);
			    }
			}
* 重点
	* 序列化的成员变量也需要实现Parcel接口，同时读取以及写入方式如下
	* 读取
		>book = in.readParcelable(Book.class.getClassLoader());
	* 写入
		>dest.writeParcelable(book, flags);
* 传递对象时  
![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-8/68062237.jpg)
* 接收对象时  
![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-8/50517350.jpg)
