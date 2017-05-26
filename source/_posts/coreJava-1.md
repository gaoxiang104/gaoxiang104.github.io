---
title: CoreJava学习(1)——面向对象、类与对象
date: 2017-05-26 09:28:02
tags: CoreJava
categories: CoreJava
---

# 面向对象的三大特征

- 封装(Encapsulation)
    - 对外部不可见。可以保护程序中的某些内容。
- 继承(Inheritance)
    - 扩展类的功能
- 多态(Polymorphism)
    - 方法的重栽
    - 对象的多态性

# 类与对象

　　类是对某一类事物的描述，是抽象的、概念上的定义；对象是实际存在的该类事物的每个个体，因而也称实例(Instance);

# 类的定义与对象的创建
```java
// 声明类
class Person {
	// 类中有两个属性
	String name;
	int age;

	// 无参构造器
	public Person() {
	}

	// 带参构造器
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	// 类中的方法
	public void tell() {
		System.out.println("姓名:" + name + ",年龄:" + age);
	}
}

public class ClassDemo01 {
	public static void main(String[] args) {
		// 声明对象，并使用`new`实例化对象
		Person person = new Person("Foo", 11);
		person.tell();
	}
}
```

# Java中对象内存划分

类属于引用传递类型，引用数据类型必然存在栈内存-堆内存的引用关系。

## 内存划分：对象创建之初
![](/assets/images/coreJava-1/1.jpg)

- 声明对象：Person per,栈内存中声明的，只开辟了栈内存的对象是无法使用的，必须有其堆内存的引用才可以使用。
- 实例化对象：new Person()，在堆中开辟空间，所有的内容都是默认值。
    - String：是一个字符串，本身是一个类，就是一个引用数据类型，则此时默认值就是null。
    - int：是一个整数，本身是一个数，所以是基本数据类型，则此时的默认值是0。

## 内存操作：为属性赋值

![](/assets/images/coreJava-1/2.jpg)

对象是保存在栈内存之中，属性是保存在堆内存之中。

## 内存操作：声明并创建多个对象

![](/assets/images/coreJava-1/3.jpg)

## 内存操作：声明两个对象，并指向同一个堆内存

![](/assets/images/coreJava-1/4.jpg)

所谓的引用数据类型，实际上传递的就是堆内存的使用权，可以同时为一个堆内存空间定义多个栈内存的引用。

## 垃圾产生的内存关系

![](/assets/images/coreJava-1/5.jpg)

因为per2改变了指向，所以其原本的内存空间就没有任何栈的引用，则这样的内存就被称为垃圾，等待这垃圾收集机制进行回收。

**`GC`**：垃圾收集机制的简称。