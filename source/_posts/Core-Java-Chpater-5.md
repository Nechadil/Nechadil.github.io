---
title: Core java (Chpater 5)
date: 2018-07-12 19:11:43
tags:
	- Core java
---
### Chapter 5 Inheritance
#### 5.1 Class, Superclass, Subclass
* override
	* 子类重写方法的时候对于父类私有域需要用*super.getter()*获取
	* super不是引用，而是一个引导compiler调用父类方法的关键字
* constructor
	* 因为子类没有访问父类属性的权限，所以必须通过父类的构造器，即super()或super(args)
	* super构造器必须是构造器的第一行（只有把父类的东西初始化了才能再初始化子类的多余部分）
	* 关于super 和 this 的比较
	Recall that the this keyword has two meanings: to denote a reference to the implicit parameter and to call another constructor of the same class. Likewise, the super keyword has two meanings: to invoke a superclass method and to invoke a superclass constructor. When used to invoke constructors, the this and super keywords are closely related. **The constructor calls can only occur as the first statement in another constructor.** The constructor parameters are either passed to another constructor of the same class (this) or a constructor of the superclass (super).
	两者的作用都是1. 调用方法 2. 调用构造器
* polymorphism & dynamic binding
	敲黑板了。这个第一次法国实习前公司的面试里一道我没答上的愚蠢题目。我当时还问这个词英文是什么。转眼间不到一个月实习就要结束了。
	*polymorphism: the fact that a variable can refer to multiple actual type*
	*dynamic binding: automatically selecting the appropriate method at run time*
*  Inheritance hierarchies
*  Inheritance hierarchies: 多个类继承同一个类
* inheritance chain：在hierarchies中从某个类到其祖先类的路径
* liskov substitution principle 里氏替换原则
	* 当需要使用父类对象的时候都可以使用子类对象
		比如：可以将父类对象赋值为子类对象的实例
		即父类可以引用（refer to）任何其子类对象。
	*	注意下面这个例子
	```
	super[] superArray；
	superArray[0] = subObject;
	//false
	superArray[0].subMethod();
	//true
	subObject.subMethod();
	```
	从编译器的角度考虑。数组superArray中的元素是super类，不能使用子类方法。
* Dynamic binding实现方式
	1. 假设调用c.f(param)，编译器寻找所有c类和c的父类中符合条件的方法：
		* 可访问
		* 方法名为f
	2. overloading  resolution
	匹配参数类型（检验签名）。如果只有一个，则调用该方法
	3. static binding
	private, static, final方法编译器直接绑定（因为这三种不存在同签名方法或是与类的继承无关），不需要区分，此为静态绑定。
	否则，调用方法由explicit parameter（即输入的参数param）的实际类型决定，在runtime时动态绑定。
	4. JVM根据c的类型决定。c的类中没有则找c的父类。
	JVM存在method table存放所有签名。每次调用类JVM去查看签名。
* 多态好处
	extensible：superClass.method() 。当新加入一个superClass的子类并将superClass赋值为一个子类对象的时候，由于使用多态而不需要更改之前的代码
* 避免继承
	* final 类：不能被继承
	* final方法：不能被override
*  casting
	子类转父类不需要casting
	父类转子类则需要（因为内容增加了）
	可能在运行时出现ClassCastException. 因此需要**instanceof**判断：
	```
	Superclass super;
	if（super instanceof Subclass）
		Subclass sub = (Subclass)super;
	```
	null instanceof Class 总是返回false（因为null不引用任何类）
* abstract class
当一个类需要作为其他类的基础而不需要使用它的实例的时候，使用抽象类。抽象类可以定义**具体方法**或者**抽象方法**。当没确定具体实现时使用抽象方法。
```
abstract class Person {
	private String name;
	public Person(String n) {
		name = n;
	}
	public abstract String getDescription();
	public String getName() {
		return name;
	}
}
```
	子类实现全部抽象方法/子类没实现全部抽象方法（**此时子类也必须为abstract**）
* Protected access
	private, public, default(package visible), protected(default + subclasses)

#### 5.2 object
* every class extends object
only primitive types are not objects
* equals()
	* default: reference equals
	* implementation:
		0. *If implement equals() for subclass, test super.equals(other) first*
		1. this == otherObject ⇒ true
		2. otherObject == null ⇒ false
		3. getClass != otherObject.getClass() ⇒ false
		4. class cast:  Class other = (Class) otherObject;
		5. return a.attribute1 == other.attribute1 && ......
	* Object.equals(attribute, other.attribute) for non-primitive classes (in case attribute == null)
* Equality testing and Inheritance
	不要使用instanceof 来代替3的步骤。问题出在有subclass的时候。instanceof会通过子类（但getClass不会）。
	* Arrays.equals(a，b) 测试数组元素是否相同
	* 注意equals方法参数类型是**Object**!!!否则方法不是重写Object类的equals方法而是新建了另一个。
* hashCode()
	* Object类hashCode()方法默认是根据对象在内存的地址
	* From java7: Objects,hashCode(object) 
		explicit parameter 为null返回0，否则调用object的hashCode方法 
	* 更简单的方法： Objects.hash(param1, param2, param3, ...)生成hashCode
	* x,equals(y) ⇒ x.hashCode() == y.hashCode()
	* **重写hashCode方法的原因**：在使用HashMap的时候，两个相同的key值应该返回相同的内容。如果不重写hashCode，Object类里的方法计算的是内存地址，一般不同。导致相同key值无法获得相同结果。
	* Arrays.hashCode（）方法返回根据数组元素hashCode生成的hashCode
* toString()
```java
ClassA a = new ClassA();
//a.toString() is called
String str = "..." + a;
```

#### 5.3 Generic Array Lists
*	ArrayList<Employee> staff = new ArrayList<>();
	* *<>*: diamond syntax
	* ensureCapacity(100): 在达到100前不扩容（提高了效率）
*  Accessing elements
	* array.set(i, element);   等价于 array[i] = element;
		set只用来修改。添加用add
	* array,get(i);

#### 5.4 Wrapper and Autoboxing
* Wrapper
Interger & int
* Autoboxing (from java 5)
by the compiler
	```
	//autoBoxing
	list.add(3); ⇐ ⇒ list.add(Integer.valueOf(3));
	//unbox
	int n = list.get(i) ⇐ ⇒ int n = list.get(i).intVlaue();
	```	

#### 5.5 Methods with a variable number of parameters
* signature:
	public void method(ClassA... params)
* params 类型为 ClassA[ ]

#### 5.6 Enumeration Classes
	```java
	public enum Size {
		//每个类型只有一个实例
		Size1(attr1), Size2(attr2)...;
		//实例的值，为私有属性
		private AttrClass attr;
		//实例化方法（私有方法，用于初始化）
		private Size(AttrClass attr) {this.attr = attr:}
		//公有getter
		public AttrClass getAttr(){
				return attr;
		}
	}
	// 获取属性值方法
	Size.Size1.getAttr();
	//返回Size.Size1, Size,Size2, ......
	Size[] values = Size.values();
	```
	
#### 5.7  Reflaction
*  *Class*类
	* runtime type identification 
		用来跟踪每个对象所属的类，JVM由此选出属于该对象的正确方法
		JVM针对每种类型只有一个Class对象，所以可以直接比较内存地址
	```java
	String name = object.getClass().getName();
	Class cl = Class.forName("java.util.Date");
	//认证类的种类
	if(e.getClass() == AClass.class) {
		......
	}
	//调用无参构造函数创建新实例,如无此函数则抛出异常
	e.getClass().newInstance();
	Class.forName("java.util.Date").newInstance();
	```
* 类加载顺序
	包含main方法的class先启动，加载它需要的所有类。这些类加载它们依赖的类。
	* Class c = object.class
* 使用反射
	* java,lang.reflect包：Field, Method, Constructor
* Generic Array
* Invoke

#### Design hints for  Inheritance
* 
	
	
	 	







> 学习笔记
> 07/07/2018 （ P200 ~ P211）
> 周六一天只看了11页。。。至少早起了。而且在非工作日学习了。需要继续努力。好的开始。
> 08/07/2018 （ P211 ~ P221）
> 洲际赛LPL险胜，看得惊心动魄。继续努力才能安心看S8全球总决赛。你看看人家职业选手，doinb，mouse这种的，都是下放到各层联赛摸爬滚打好几个赛季才有所成，职业精神值得学习。
> 09/07/2018 (P222 - P249 )
> 11号记录。昨天晚上睡了会儿觉看了场世界杯。白天搞加密的学习也没看书。今天忽然理解坚持确实比数量要重要的多。每天10页三个月内就能搞定，20页更是一个月就完事儿了。但是如果不坚持下来很可能到最后连一本书都完成不了。不要贪快，学习乐在其中才是最重要的。每天10页书保底，一定要坚持下来。
> 12/07/2018 (P250 ~ P271)
> 用上班时间把这章最后一部分看完了。反射的部分后面有些东西就没记了。了解了一下能做什么，等到有需要的时候再回过头来学习吧。到目前为止记笔记的方式始终是边看边记。在考虑要不要做得更精致点。从下一章开始先浏览一定量的内容（比如一个小节），然后回过头来再整理。