---
title: CoreJava学习(2)——Java的封装性
date: 2017-05-26 14:05:26
tags: CoreJava
categories: CoreJava
---

# 为什么要有封装
- 封装就是保护内容。
- 保证某些属性或方法可以不被外部看见。

```java
class Person {
    String name;	
    int age;	
    public void tell() {
        System.out.println("姓名：" + name + "，年龄：" + age);
    }
}

public class EncDemo01 {
    public static void main(String[] args) {
        Person per = new Person();
        per.name = "Foo";
        per.age = -30;	
        // 将age设置为-30,语法上没有错误，但是从实际的角度看，是错误的。
        per.tell();
    }
}
```

以上的代码，语法上没有问题，但是从实际的角度看，以上的代码绝对不符合要求。
不合适的根本原因在与此处让属性可以直接被外部访问。

# 实现封装

 将类中的属性设置为私有权限（**`private`**）,使得外部无法直接调用。

``` java
class Person {
    private String name;
    private int age;
    ...
}
```

## getter()和setter()方法
    
因为这些属性肯定是要表示一些实际的意义，那么这些有意义的内容肯定应该由外部设定，所以在java中对于封装性的访问就给出了一个明确的原则，此原则必须遵守。

被封装的属性如果需要被访问，则需要编写setter及getter方法完成。

例如：现在有一个属性，private String name;
- Setter（设置）：public void setName(String name);
- Getter（取得）：public String getName();

``` java
class Person {
    private String name;
    private int age;

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        if(0>=age && 150 <= 150 ){	//加入验证
            this.age = age;
        } else {
            System.out.println("Illegal age field");
        }
    }

    public void setName(String name) {
        this.name = name;
    }

    public void tell() {
        System.out.println("姓名：" + name + "，年龄：" + age);
    }
}
```

