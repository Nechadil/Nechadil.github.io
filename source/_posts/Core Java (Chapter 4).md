---
title: Core Java (Chapter 4)
date: 2018-07-06 18:17:37
tags:
	- Core Java
---
### Chapter 4 Objects and Classes
#### 4.1 Intro
* programming paradigm: oop vs structured procedural programming
	* structured: algo + data structures
	* oop: data structures + algo
* Big problems:
	* 2000 procedures + global data
	* 100 classes * 20 methods + Object data
* Encapsulation:
	* combine data + behavior in one package
	* hide implementation details from user of the object
	* access instance fields by methods
* Relations between classes
	* Dependence (use - a): minimize the coupling
	* Aggregation (has - a)
	* Inheritance (is - a)
#### 4.2 Using predefined classes
* Object variable **refers** to an object (not _contains_)
* Date: a point in time
	GregorianCalendar: dates in the gregorian calendar notation
* mutator methods: change instance fields (setter, ...)
	accessor methods: only accesss instance fields (getter)
#### 4.3 Define a Class
* implicit param: object before the method (_this_)
	explicit param: params in the signature of method
* getter & setter vs. public attribut:
	* change the internal implementation whithoud affecting any code other than the methods
	```Java
	//Old version
	private String name;
	public String getName(){
		return name;
	}
	//New version
	private String firstName;
	private String lastName;
	//without getter: change everywhere that used aObject.name
	aObject.name ⇒ aObject.firstName + AObject.lastName;
	//with getter: only need to change the getter
	public String getName(){
		return firstName + lastName;
	}
	```
	* do the error check or convert inside the getter/setter
* **Do not** return a mutable object
	```Java
	private Date date;
	//return a Date
	public Date getDate(){
		return date;
	}
	//date attribut can be modified, so breaks encapsulation!!
	d = aObject.getDate();
	d.setTime(blabla);
	//do this way:
	public Date getDate(){
		return date.clone();
	}
	```
* **a method of a class is permitted to access the private fields of any object of this class**
* final
	* must be initialized when the object is constructed (in constructure or declairation)
	* final + primitive type /immutable class
	* final method/attribut : can't be overwrite/modified
#### 4.4 static fields and methods
* static
	* belongs to class not object
	* only one copy for 0 to any number of objects
* static constants
	* static final 
* static method
	* no implicit param
	* has access to static field of the same class
* Factory method
	* static method
	* creates various objects from a same class
#### 4.5  method parameters
* call by value: just get the value passed
	call by reference: method gets the _location_ of the 	variable the caller provides
	* java **always** uses call by value 
	* _object references are passed by value_（swap方法不起作用）
	* **A method cannot modify a parameter of a primitive type (that is, numbers or boolean values).**
	* **A method can change the state of an object parameter.**
	* **A method cannot make an object parameter refer to a new object.**
	* 最后说一句，就是Java是值传递，但是对于对象类型传递的值是一个引用。所以调用引用对象的方法是可以改变对象state的，但是没法改变引用的对象，因为传的是值，并不是引用。
#### 4.6 Object construction
* overloading: sevral methods have the same name but different parameters.
* Constructor with no arguments: 
	* a free no-argument constructor only when your class has no other constructors  (otherwise create it if needed)
* calling another constructure with _this_
	```Java
	public Employee(double s)
	{
		// calls Employee(String, double)
		this("Employee #" + nextId, s);
		nextId++;
	}
	```
* **Initialization Blocks**
	* **Class declarations can contain arbitrary blocks of code. These blocks are executed whenever an object of that class is constructed.**
	* The initialization block runs first, and then the body of the constructor is executed.
	* 1. All data fields are initialized to their default values (0, false, or null).
		2. All field initializers(在属性声明时初始化) and initialization blocks are executed, in the order in which they occur in the class declaration.
		3. If the first line of the constructor calls a second constructor, then the body of the second constructor is executed.
		4. The body of the constructor is executed.
	* static block
		```Java
		//executed when the class is first loaded
		static{
			//initialize static fields here
		}
		```
	*	All static field initializers and static initialization blocks are executed in the order in which they occur in the class declaration. （注意这个是类加载时执行，所以比类的对象生成的时间要早）
	*	Destruction and *finalize*
	*	*finalize* method
		* finalize() may be invoked on an object when it becomes garbage. Object's implementation of finalize() does nothing—you can override finalize() to do cleanup, such as freeing resources.
		*	The finalize() method may be called automatically by the system, but when it is called, or even if it is called, is uncertain. Therefore, you should not rely on this method to do your cleanup for you. For example, if you don't close file descriptors in your code after performing I/O and you expect finalize() to close them for you, you may run out of file descriptors.
#### 4.7 Packages
*  reason: garantee the uniqueness of class names
* **compiler locates classes in package**: bytecodes in class file classes are referred in full package
* import static
	```Java
		//importing not just classes but also static methods/fileds
		import static java.lang.System.*;
		out.println("Goodbye, World!"); // i.e., System.out
	```
* compile .java file of class in package a.b.c ⇒ .class file in subdirectory a/b/c
* Package scope(范围)
	* private public
	* default: accessible in the same package
#### 4.8 Class path
* use .jar file
	* set **class path**
		1. class path is the collection of all locations that can contain class files
			格式：地址a:(windows中是;)地址b:地址c
			此处地址可以是文件路径（如：/home/user/classdir或**.**代表当前文件夹）
			也可以是jar包路径（如；/home/user/archives/archive.jar）
		2. Java 6开始支持通配符，如*home/user/archives/'\*'*代表archives下所有**jar包**（而不是class文件）
		3. runtime libraray files(jre/lib 和 jre/lib/ext)不需要导入
		4. compiler： 始终会在当前文件夹寻找
			JVM：只有*.*在classpath时才会在当前文件夹寻找类。class path默认含当前文件夹（即不设置的时候）。如果设置了而不含当前文件夹，则编译通过但运行不一定通过。
		5. JVM寻找类的过程：
			* 条件： class path = /home/user/classdir:.:/home/user/archives/archive.jar
							要找的类：com.horstmann.corejava.Employee
			* 在jre/lib 和 jre/lib/ext中找
			* 找/home/user/classdir/com/horstmann/corejava/Employee.class
			* 在当前文件夹找 com/horstmann/corejava/Employee.class
			* 在archiver.jar中找 com/horstmann/corejava/Employee.class
		6. compiler确定类全名的过程（注意JVM读取class文件。而class文件里类都是全名，所以不存在这一过程。只有compiler在将java文件转化成class文件时才需要定位引用文件的位置）：
			* 如果类包含package全名： 结束
			* 否则按下列方式执行。条件：
			```Java
			 import java.util.\*;
			 import com.horstmann.corejava.\*;	
			 Employee e = new Employee();
			```
			*	先找java.lang.Employee (因为java.lang包总是默认导入)
			*	再找java.util.Employee
			*	com.horstmann.corejava.Employee
			*	在当前包中找Employee
			*	*找到多个相同类报compile error*
			*	*compiler比对源代码和class文件的版本。如class文件版本落后则更新*
#### 4.9 comments
		(maybe) see later
#### 4.10 Class design hints
>#### 学习记录
>##### 05/07/2018
>P134 - P182
按这个进度大概九月份才能读完（还是在每天学习的情况下）。恩，现在不是讨论早晚的问题，而是先坚持下去。在上班没其他事情的情况下进度也不是很快，主要是有些地方还是挺需要动脑的。今天这一部分最主要的是类加载机制和Java的值传递。
>##### 06/07/2018
P183 - P199
干货比较多。比如package，classpath。注意compiler，JVM对于类加载的机制。
还有就是今天才发现外部类只有public和default两种。为什么没有private？因为private类无法被外部访问，没意义。为什么没有protected？因为protected只能被同包或子类访问。前者即是default，后者就意味着一个问题，如何控制该包能被什么类继承？如果所有类都能继承，即所有类都是子类，即所有类都能访问该类，那该类就是public。如果没有类可以继承，那该类就是default。如果只有部分类可以继承。那是哪一部分呢？无法指定。所以类的访问权限（而不是方法）最小就是同包。
还有就是一个Java文件只允许一个public类，同时文件名必须与类名相同。没在网上确认，我的理解是这在import的时候很有用，同时涉及JVM加载类机制。如果一个包使用另一个类，JVM就要去class path里通过文件系统找这个类。如果类名和文件名不同这个过程就会很复杂。
第四章终于在班上结束了。之后回家要开始第五章的学习了。