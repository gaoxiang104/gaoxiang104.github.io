---
title: CoreJava学习(4)——String类
date: 2017-05-27 13:07:51
tags: [CoreJava,String]
categories: CoreJava
---

# 1.String类的两种实例化方式

直接赋值：
```java
public class StringDemo01 {
    public static void main(String[] args) {
        String name = "Foo";
        System.out.println("name:" + name);
    }
}
```

通过关键字new
```java
public class StringDemo2 {
    public static void main(String[] args) {
        String name = new String("Foo");
        System.out.println("name:" + name);
    }
}
```

# 2.String的两种比较操作

使用“==”进行比较:
```java
public class StringDemo3 {
    public static void main(String[] args) {
        String str1 = "hello";  // 直接赋值
        String str2 = new String("hello");  // 通过new赋值
        String str3 = str2; // 传递引用
        System.out.println("str1 == str2 --> " + (str1==str2)); // false
        System.out.println("str1 == str3 --> " + (str1==str3)); // false
        System.out.println("str2 == str3 --> " + (str2==str3)); // true
    }
}
```

下面进行内存的分析：

![](/assets/images/coreJava-4/1.jpg)

使用"=="进行比较，实际是判断地址空间是否相等，判断的是地址值。

## 2.1.String比较：equals()

如果要判断两个String的内容是否相等，则必须使用String类中提供的equals()方法完成。代码如下：
```java
public class StringDemo4 {
    public static void main(String[] args) {
        String str1 = "hello";  // 直接赋值
        String str2 = new String("hello");  // 通过new赋值
        String str3 = str2; // 传递引用
        System.out.println("str1 equals str2 --> " + (str1.equals(str2))); // true
        System.out.println("str1 equals str3 --> " + (str1.equals(str3))); // true
        System.out.println("str2 equals str3 --> " + (str2.equals(str3))); // true
    }
}
```

**结论：String的两种比较方式：**
- 一种是使用“==”，比较的是地址值
- 另一种是使用“equals()”方法，比较的是具体的内容

# 3.两种实例化方式的区别
在String中可以使用直接赋值和new调用构造方法的方式完成，那么该使用那种更合适呢？

## 3.1.一个字符串就是String的匿名对象
```java
public class StringDemo5 {
    public static void main(String[] args) {
        // 直接赋值的字符串"hello" 可以调用equals()方法
        // 说明一个字符串就是一个String的匿名对象
        System.out.println("hello".equals("hello"));
    }
}
```

直接赋值的字符串"hello" 可以调用equals()方法。 说明一个字符串就是一个String的匿名对象。

## 3.2.String类两种实例化方式的区别——直接赋值
![](/assets/images/coreJava-4/2.jpg)


## 3.3.String类两种实例化方式的区别——通过关键字new赋值
![](/assets/images/coreJava-4/3.jpg)

使用直接赋值的方式只需要一个实例化对象即可，而使用new String()的方式则要开辟两个内存对象。
开发中最好使用直接赋值的方式完成。

# 4.字符串的内容不可改变
![](/assets/images/coreJava-4/4.jpg)

实际上字符串内容的改变，改变的是内存地址的应用关系。