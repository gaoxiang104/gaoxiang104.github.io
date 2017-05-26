---
title: CoreJava学习(3)——构造方法与匿名对象
date: 2017-05-26 15:08:02
tags: CoreJava
categories: CoreJava
---

# 构造方法的概念及调用时机

构造方法的定义格式：
``` java
class 类名称 {
    访问权限 类名称 (类型1 参数1,类型2 参数2,...){
        ... // 构造方法没有返回值
    }
}
```

**在构造方法的声明中需要注意以下几点：**

- 构造方法的名称必须与类名一致
- 构造方法的声明处不能有任何返回值类型的声明
- 不能在构造方法中使用return返回值

## 调用时机
java中实例化对象时调用构造方法。
测试代码：
```java
class Person{
    public Person(){
        System.out.println("create a Person object");
    }
}

public class ConstructorDemo {
	public static void main(String[] args) {
        System.out.println("声明对象");
        Person per = null;
        System.out.println("实例化对象");
        per = new Person();
    }
}
```
运行结果：
```shell
声明对象
实例化对象
create a Person object
```

## 默认的无参构造

**注意：**
- 每个类中肯定都会有一个构造方法。
- 如果一个类中没有声明一个明确的构造方法，则会自动生存一个无参的什么都不做的构造方法。

```java
class Person{
    public Person(){
        
    }
}
```

# 构造方法的重载

构造方法的重载过程与普通方法一样：**`参数的类型或个数不同`**

```java
class Person{
    private String name;
    private int age;    
    public Person(){
        
    }
    public Person(String name){
        this.setName(name);
    }
    public Person(String name,int age){
        this.setName(name);
        this.setAge(age);
    }
}
```


# 匿名对象的使用
在Java中如果一个对象只使用一次，则就可以将其定义成匿名对象。
匿名对象只开辟了堆内存。

``` java
class Person{
    public Person(){
        
    }
    public void tell(){
        System.out.println("tell()");
    }
}
public class ConstructorDemo {
	public static void main(String[] args) {
        new Person().tell();
    }
}
```