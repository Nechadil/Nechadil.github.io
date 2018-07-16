---
title: Core Java (Chpater 6)
date: 2018-07-13 19:12:23
tags:
	- Core Java
---
### Chapter 6 Interfaces and Inner Classes

#### 6.1 Interfaces
* Accessibility
	all methods are *public*
	all fields are *public static final*
	不要写访问符
* 一个interface变量要指向一个具体的实例
#### 6.2 Cloning
* protected Object.clone( )
	* mutable 类型克隆的结果还是指向同一对象的引用，所以需要重写。
	* 因为是protected所以只能克隆自己的对象。如果属性包括其他对象则不能克隆。
* 使用Cloneable接口进行deep clone
	* 实现clone方法，访问权限为public。
	* 方法内对调用自身属性的clone方法
#### 6.3 Interfaces and Callbacks
* 这种callback原理就是在功能类中参数是接口而不是具体实现。在实际使用时传入一个该接口的实现类。这样降低了类之间的耦合。
#### 6.4 Inner Classes
* 为什么要用内部类？
	* 可以直接使用外部类private域
		* 可访问外部类和自身域
		* 编译器自动在内部类构造方法中添加外部类引用来实现对外部类的访问
	* 其他类不可见
		* 只有内部类才能使用private访问符
		* 如果内部类为public，可这样新建内部类：
		```Java
		OutterClass outerObject = new OutterClass();
		InnerClass innerObject = outerObject.new InnerClass();
		```
		用此可见内部类的创建需要包含一个外部类的实例。
	* 匿名内部类方便定义callback
* 编译器对内部类的实现
	* 就是简单的一个类，名为InnerClass$OuterClass
	* 内部类多出一个域用来引用外部类：final OuterClass **this$0**；
	* 内部类访问外部类域：
		* 外部类多出一个静态方法：static Type  **access$0**(OuterClass o);
		* 访问时执行access$0(this$0);
	* private InnerClass实现：default访问权限 + private构造方法
* Local Inner Class
	* 在方法内定义Class
	* **不带访问符!!!** 方法内有效
	* 优点是甚至对外部类隐藏。只有外部方法才能访问
	* 可访问方法的参数，但参数在方法中必须声明final。原因：
		该参数会被编译器加在内部类中作为一个属性。
		需要保证该属性和参数在引用同样的数据，所以要定义成final，即不能被更改。
	* **数组被定义为final仍可以更改其中的数据**
* Anonymous Inner Class
	新建一个匿名内部类实例
	```Java
		// 新建一个继承SuperClass类的匿名内部类。
		new SuperClass(construction parameters) {
			inner class methods and data
		}
		// 新建接口实现没有参数
		new Interface() {
			inner class methods and data
		}
		// 匿名内部类没有构造方法，如果是第一种继承父类的内部类，需使用父类的构造方法
	```
	* double brace initialization
	```Java
		// 外层中括号表示匿名内部类，内层中括号是对象初始化时执行的代码块
		ArrayList<String> a = new ArrayList<String> {{add("a");add("b");}} ;
	```
* static inner class
	* 用于不需要访问外界对象的内部类。
	* 只为外部类服务（比如一个类中一个返回最大值最小值的方法，新建一个静态内部类Pair，储存最大最小值，只用于这个类，没必要在包内新建一个类）
	* 接口内的内部类都是自动public static的
#### 6.5  Dynamic Proxy
书里讲的完全看不懂。。。但是在网上找到了[这篇文章](http://www.cnblogs.com/xdp-gacl/p/3971367.html)，教你如何找刘德华唱歌跳舞。写得很好。
* 总的来说分三个部分。一个是接口，比如“会表演”。一个实现类，比如“演员”。一个代理类，比如“经纪人”
* 在经纪人方法内用Proxy.getNewInstance(ClassLoader classLoader,Interface[] interfaces,InvocationHandler handler)方法获取演员的代理，方法内的interface即为实现类实现的接口。
* InvocationHandler实例即为具体的处理者**接口**。实现InvocationHandler时需要重写invoke(proxy, method, args)方法。其中method即为调用的接口的某一方法。可在method.invoke()前后增加操作（如果不加method.invoke()则实现类的方法不再执行）



> 学习笔记
> 12/07/2018 (P272 ~ P299)
> 花了很多时间在单位学习，已经到了300页了。之前估计在内部类那个部分即使是汉语版也看不太懂。现在已经可以明白作者在说什么了。之后如果有相关经验再回过头来看应该会更清晰了。
> 13/07/2018（P300 ~ P314 ）
> arrayList的初始化jeremie 之前在代码里用过，原来是这么回事啊！之前还以为是什么特殊的语法糖。