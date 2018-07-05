---
title: Core Java Chapter 4
date: 2018-07-05 18:17:37
tags:
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
*  Date: a point in time
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
#### 4.4 static fields and methods
* static
	* belongs to class not object
	* only one copy for 0 to any number of objects
* static constants
	* static final 
P164
