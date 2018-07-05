---
title: Core Java (Chapter 3)
date: 2018-07-05 19:37:40
tags:
	- Core Java
---
很惭愧地上网下了一个英文版pdf。想想两三年前就买了这本书的中文版，就又有些许的心安理得。重新看一遍吧。之前看了好几回都没能通过第四章的样子。如今两年过去做了些项目，应该会容易些。争取在年末前先过一遍。
前两章略过了，直接从基础概念看起。

### Chapter 3
#### 1. 基本类型
* 整形（byte，short，int，long）：存储空间，范围
* 浮点（float，double）：单双精度。使用BigDecimal类进行精确计算。
* 字符(char)：2bytes
今天下午正好也在研究字符的东西。Java里的char是表示UTF-16 encoding下的code units。比如: System.out.println("\uD835\uDD6B")的结果是zz。注意一个code point是指在Unicode的code page中字符与代码的映射。在不同的caracter encoding下（UTF-8，UTF-16）一个code point对应不同数量的code unit。
#### 2. 变量
* final 不可变量
使用static final在类中声明供所有内部方法使用。
#### 3. 类型转换
原则是不丢失信息。比如byte => short => int =>long  
#### 4. enumerated type
```Java
enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE }
Size s = Size.MEDIUM;
```
#### 5. 字符串
* substring 
```Java
String greeting = "Hello";
String s = greeting.substring(0, 3);//'Hel'(0~2)
```
这种方式优点是两个参数相减长度即为字符串长
* immutable
关于动机几乎类似月经贴了，总是记不住。
首先字符串的改变本质是引用对象的改变。改变后之前字符串的**值**是不可变的。
为什么这样做？一方面这种方式不能直接修改字符，降低了效率。另一方面使字符串保存在常量池里，可以被共享，提高了效率。
* Code point & Code unit
```Java
char ch = "𝕫".charAt(1)；//返回的是这个Unicode code point在UTF-16下的第二个code unit值
```

 所以使用char是很危险的，因为supplementary characters是用一对儿code unit来表示而不是一个。而一个char表示一个code unit。
```Java
//前序遍历字符串
int cp = sentence.codePointAt(i);
//Characters whose code points are greater than U+FFFF are called supplementary characters.
if (Character.isSupplementaryCodePoint(cp)) i += 2;
else i++;

//后序遍历字符串
i--;
/**
Determines if the given `char` value is a Unicode surrogate code unit.
Such values do not represent characters by themselves, but are used in the representation of [supplementary characters](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#supplementary) in the UTF-16 encoding.
A char value is a surrogate code unit if and only if it is either a low-surrogate code unit or a high-surrogate code unit.
**/
if (Character.isSurrogate(sentence.charAt(i))) i--;
int cp = sentence.codePointAt(i);
```
注意两种遍历一种是以code point为单位，一种是以code unit为单位。
* 字符串的连接
（原来从开头就出现了面试基础知识。看来不是面试题难，而是书看的确实不够。真的都是最基础的点了。）
```Java
//好处是避免了多个String对象的创建
StringBuilder builder = new StringBuilder();
builder.append(ch); // appends a single character
builder.append(str); // appends a string
String completedString = builder.toString()；
```
这里还有一个月经问题就是StringBuffer和StringBuilder的区别。StringBuffer是线程安全的，实现原理是在某些方法上加了synchronize关键字。但是StringBuffer好像没什么人用了，因为线程安全对于字符串而言好像没什么意义。

#### 6. 输入输出
* 输入
```Java
//console
Scanner in = new Scanner(System.in);
String aLine = in.nextLine();
String aWord = in.next();
int aInt = in.nextInt();
//fileIO
Scanner fileInput = nez Scanner(Path.get("x.txt"));
PrintWriter out = new PrintWriter("output.txt"); 
```
#### 7. control flow 
* do while
* switch
	```Java
	switch(var) {
			case var1:
				...
				break;
			case var2:
				...
				break;
			...
	}
	```
	注意没有break则会继续进行判断。作者评价如下：
	> This behavior is
plainly dangerous and a common cause for errors. For that reason, we never use the switch
statement in our programs.
* break control flow
	```Java
		// break out of loop
		while(tset){
			...
			break;
			...
		}
		// break out of multiple nested loop
		// label + colon
		break_label:
		while{
			for(){
				if(){
					// out of all the loops
					break break_label;
				}
			}
		}
		//jumps here
	```
* array
	* copy array
	```Java
		int[] arrayA = arrayB;
		//modified  also the arrayB
		arrayA[3] = 1;
	```
	注意Java是传递引用。所以如果想新建数组使用Arrays.copyOf(arrayB, arrayB.length)方法。
	* 扩容
	```Java
	Arrays.copyOf(arrayB, arrayB.length*2);
	```	
	*	彩票抽奖
	程序就不贴了。这里边的问题是如何从一个数组中取出几个不重复的随机数。要点是每次随机的数不是值而是index。这样当这个index里的值被保存后把最后一个值传到index里，同时把取值范围减一。这样最后一位就不会被计入下次抽取中，同时把上一个抽取到的值从数组中抹去。


### 学习记录
#### 2018/06/28（P59~P90）
真的是常看常新。三年前快冬天的时候我还记得看这一章觉得很简单，其实里边内容很多。要见识的多了再回头看才能有更多收获。
#### 2018/07/02 (P91-P112)
进度差了一些。
#### 2018/07/03(P113-P134)
这个是4号记的笔记了。总体来说进度感觉有点慢。尝试要不要简略地过一遍然后把一个脉络记下来。但是总算完成了一章的学习。继续努力。