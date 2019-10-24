---
title: android动画简介
date: 2016-10-10 19:35:56
tags:
- android

---
### ViewAnimation
#### **视图动画**
#### 基类是Animation，包含的子类有
* AlphaAnimation 透明度动画
* TranslateAnimation 平移动画
* ScaleAnimation 缩放动画
* RotateAnimation 旋转动画
* AnimationSet 动画集合
<!--more-->
#### 定义方法
* java代码
* xml文件定义  
	* xml文件
		>  res->anim->view_alpha.xml
	 
	* java文件  

			Animation animation = AnimationUtils.loadAnimation(getApplicationContext(), R.anim.view_alpha); 
<!--more-->
### **PropertyAnimation**属性动画

* [google官方api](https://developer.android.com/guide/topics/graphics/prop-animation.html)
#### 与视图动画的区别
* 视图动画比较局限,仅能够修改view对象,局限于view对象的一些特定属性:例如scale,alpha等.
* 视图动画仅仅是修改了view绘制的地方而非view本身的属性.(在你移动一个按钮到新的位置时,点击新的位置的按钮并不能够触发事件监听)
#### ValueAnimator
* 使用:

		ValueAnimator animation = ValueAnimator.ofFloat(0f, 1f);
		animation.setDuration(1000);
		animation.start();
	* 一般不直接使用这个,因为其只能够修改值而不能够将其值作用于目标上,这个类一般是作为下面的ObjectAnimator的内部实现.
	* 常用的几个方法为ofFloat ofInt ofArgb ofObject.
	* ofObject时需要传递实现了TypeEvaluator接口的类以提供计算方法.[TypeEvaluator文档](https://developer.android.com/reference/android/animation/TypeEvaluator.html)
#### ObjectAnimator
* 使用:

		ObjectAnimator anim = ObjectAnimator.ofFloat(foo, "alpha", 0f, 1f);
		anim.setDuration(1000);
		anim.start();
	* 常用的方法与ValueAnimator相同
	* 第一个参数是target,即需要进行操作的类
	* 第二个参数是property,target类中存在的变量,需要有setter方法,当setter方法不存在时:
		* 添加方法
		* 使用包装类
		* 使用valueAnimator类
	* 注意,有时候可能需要在动画的update监听器当中调用view的invalidate()方法来进行页面的重画.
* tips
	* 通过调用addListener添加动画监听器
		* ![](http://7xkzud.com1.z0.glb.clouddn.com/16-10-20/36491693.jpg)
		* 由于AnimatorListener需要实现的方法较多,所以google提供了另外一个实现了AnimatorListener接口的抽象类AnimatorListenerAdapter,你只需要实现这个类里面的某个方法即可(当你仅仅需要监听其中的某个方法的时候),其他方法默认为空实现.
#### xml定义动画
* 定义在res下的animator文件夹中
		
		<set android:ordering="sequentially">
		    <set>
		        <objectAnimator
		            android:propertyName="x"
		            android:duration="500"
		            android:valueTo="400"
		            android:valueType="intType"/>
		        <objectAnimator
		            android:propertyName="y"
		            android:duration="500"
		            android:valueTo="300"
		            android:valueType="intType"/>
		    </set>
		    <objectAnimator
		        android:propertyName="alpha"
		        android:duration="500"
		        android:valueTo="1f"/>
		</set>
* java代码
		
		AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext,
		    R.anim.property_animator);
		set.setTarget(myObject);
		set.start();
#### 还未涉及到的...
* animatorset 动画集合的使用
* xml定义动画的具体使用:当valueType时的处理方法