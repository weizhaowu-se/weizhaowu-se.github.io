---
title: Java高级程序设计之面向对象
date: 2017-08-01 21:56:36
tags:
- javascript
- 继承

---
# Java高级程序设计之面向对象
## 对象
### 使用对象字面量来创建对象
* 对于对象的每一个属性来说,,都有两种类型的属性来确实其性质:
	* 数据属性
		* Configureable 是否可配置,该属性指定对象是否可以配置(主要指的是数据属性)
		* Enumerable  是否可遍历,指定for in时是否返回
		* Writable
		* Value 属性的数据值,例如person中的属性name的值wilbert
	* 访问器属性
		* Configureable 是否可配置,该属性指定对象是否可以配置(主要指的是数据属性)
		* Enumerable  是否可遍历,指定for in时是否返回
		* Writable
		* Value 属性的数据值,例如person中的属性name的值wilbert  
		* Get  
		* Set
<!--more-->
---
	var person = {
    	name: "wilbert",
    	sayHello: function () {
    	    console.log("hello!world!")
    	}
	}
	Object.defineProperty(person, "name", {
	    get: function () {
	        return name + "test";
	    },
	    set: function (newValue) {
	     name = newValue;
	     }
	})
	person.name = "abcd";
	console.log(person.name);
	//输出 abcdtest


* 注意,仅能通过Object.defineProperty()或者Object.defineProperties()方法来对上述属性进行修改.

----
### 工厂模式
* 使用一个普通的函数,在函数中新建Object并且设定其属性,之后
返回这个对象
### 构造函数模式
	function Person(name, password) {
	    this.name = name;
	    this.password = password;
	    this.sayHello = function () {
	        console.log("Hello!");
	    }
	}
	person = new Person('a', 'a');
* 使用new操作符来创建对象
可以使用instanceof来判断对象类型
* 构造函数也可以看成普通的函数来调用,注意此时的this的作用域(一般情况下是windows)
* 缺点:每个方法都相当于重新实例化了一遍,这个问题可以通过
原型模式property解决
### 原型模式
* 每个构造函数都存在着prototype属性,这个属性指向一个原型对象,默认原型对象仅有constructor属性,constructor属性中又存在着指向构造函数的指针,如图
* ![](http://7xkzud.com1.z0.glb.clouddn.com/17-8-1/13154209.jpg)
* 而通过构造函数实例化的对象都存在一个无法访问的指针[[prototype]]指向**同一个**原型对象.
* 指针的获得方式
  
		 Object.getPrototypeof(person)

* 所以我们可以通过构造函数的prototype属性来设置原型对象从而达到公用函数或者变量的目的.
* 注意,访问一个对象的属性时的访问顺序:先访问自身的值,如果不存在,再访问原型对象中的值.
#### 动态原型模式
看代码

	function Person(name, password) {
	    this.name = name;
	    this.password = password;
	
	    if(typeof this.sayHello != "function") {
	        Person.prototype.sayHello = function () {
	            console.log(this.name + this.password);
	        }
	    }
	}

* 结合了构造函数与原型模式的优点,使其在使用上与普通的C/C++系的类更加相同.


## 继承
顺着上面的原型链继续写点关于继承的内容.
### 实现方法:
设置子类(SubType)的原型属性(prototype)为父类(SuperType)的实例.
### 大概原理
* 图片
![](http://7xkzud.com1.z0.glb.clouddn.com/17-8-2/37511968.jpg)
* 代码大致如下

		function Man(age) {
		    this.age = age;
		    this.name = "test sub";
		}
		Man.prototype = new Person("sub", "sub");
		man = new Man("10");
		man.sayHello();
		//访问Person类的方法
		console.log(man.name);
		//访问Person类的属性
* 此时情况如下:
	* man中的\[[prototype]](不可访问指针)指向Person(父类)的实例对象person
	* person类中的\[[prototype]](不可访问指针)指向Person(父类)的原型对象
	* Person(父类)的原型对象的Constructor中存在指向Person的指针
* 综上,构成了一条原型链,而根据属性的访问规则:
>访问一个对象的属性时的访问顺序:先访问自身的值,如果不存在,再顺着原型链直至查找到对象为止.

* 重点在于
	* 构造函数存在prototype指向原型对象;
	* 默认原型对象的构造函数存在指向构造函数的指针;
	* 对象实例(通过构造函数创建)存在指向原型对象的指针.
* 可以通过 instanceOf判断类型
* 给原型添加方法的代码一定要放在替换原型的语句之后.
