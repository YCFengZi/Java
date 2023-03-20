---
title: equals覆盖
date: 2023-03-20 22:10:10
tags: 学习·成长
categories: 笔记
keywords: YCFengZi
description: 个人收集网上的资料，对equals的学习理解
cover: http://rr9df52rr.bkt.clouddn.com/Model/CTy65_00G%230000.png
top_img: http://rr9df52rr.bkt.clouddn.com/AI/0.png
copyright_author: YCFengZi
copyright_info: YCFengZi出品，必属精品
sticky: 1
---

------



# **为什么要覆盖equals？**

### Equals是类Object的方法

### 作用：比较两个对象是否相等

### 源码：

```java
public boolean equals(Object obj) {
	return (this == obj);
}
```

简简单单几行，提供的作用也只有单个内存地址是否相等。

### 缺点：只是比较两个对象指向的内存地址是否相等

<p style='color: red;font-size: 20px;'>易错点：两个对象相等，但地址不同，就会返回了false



### So，这个时候就要覆盖equals

------

 

<p>重写equals就要重写hashcode（）方法？<span style='color: green;'>（待研究）</span></p>



<p>因为如果两个对象的根据equals()比较的时候是相等的，难么这两个对象一定要有相同的hash值，但是不同的对象却不一定有不同的hash值。<span style='color: blue;'>(网上的说法)</span></p>

<p>枚举类型，每个值至多只存在一个对象，即逻辑相同与对象等同是一回事，此类不需要覆盖equals方法？<span style='color: green;'>（待研究）</span></p>



------

# Equals方法覆盖的必要条件：

### 1.自反性

对于任何非null的引用值X，x.equals(x)必须返回true

（就是自己等于自己呗）

### 2.对称性

对于任何非null的引用值x和y，如果x.equals(y)返回true时，则y.equals(x)必须返回true

x.equals(y) <——> y.equals(x)

### 3.传递性

对于任何非null的引用值x、y和z，如果（x.equals(y) && y.equals(x)），则x.equals(z)也必须返回true

（x.equals(y) && y.equals(x)）——> x.equals(z)

### 4.非空性

不能与null

### 5.一致性

如果两个对象相等，则要始终相等，使用equals方法不要依赖不可靠的资源

 

# Equals覆盖的方法与步骤：

### 1.使用 == 判断是否为对象自身的应用

### 2.使用instance of判断是否为正确的类型

### 3.进行类型转换

### 4.对对象中的“关键域”进行是否相等的比较

###### **7-5 jmu-Java-03面向对象基础-05-覆盖**

```java
import java.util.Scanner;
import java.util.Arrays;
import java.util.Objects; //一定要导入这个包

public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int n1 = input.nextInt();
        PersonOverride[] persons1 = new PersonOverride[n1];
        for (int i = 0;i < n1;i++) {
            persons1[i] = new PersonOverride();
        }
        int n2 = input.nextInt();
        int n2Length = 0;
        PersonOverride[] persons2 = new PersonOverride[n2];
        for (int i = 0;i < n2;i++) {
            PersonOverride per = new PersonOverride(input.next(),input.nextInt(),input.nextBoolean());
            boolean flag = true;
            for (int p = 0;p < n2Length;p++) {
                if (persons2[p].equals(per)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                persons2[n2Length] = per;
                n2Length++;
            }
        }
        for (PersonOverride p : persons1) {
            System.out.println(p);
        }
        for (int i = 0;i < n2Length;i++) {
            System.out.println(persons2[i]);
        }
        System.out.println(n2Length);
        System.out.println(Arrays.toString(PersonOverride.class.getConstructors())) ;
    }
}

class PersonOverride {
    private String name;
    private int age;
    private boolean gender;
    
    public PersonOverride(){
        this("default",1,true);
    }
    public PersonOverride(String name,int age,boolean gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
    
    public boolean equals(Object o) {
        if(o==this) {
            return true;
        } //使用 == 判断是否为对象自身的应用
        if(!(o instanceof PersonOverride)) {
            return false;
        } //使用instance of判断是否为正确的类型
        PersonOverride personOverride= (PersonOverride) o; //进行类型转换
        return Objects.equals((this.name),personOverride.name) && this.gender == personOverride.gender && this.age == personOverride.age; //对对象中的“关键域”进行是否相等的比较
    }
    public String toString() {
        return name + "-" + age + "-" + gender;
    }
}
```

 

------

<p style='text-align: right;font-size: 20px;'>——写于23.3.20</p>
<p style='font-size: 15px'>明天的目标：<br>
&nbsp;&nbsp;&nbsp;&nbsp;<span style='font-size: 10px'>1.搞懂为什么要重写hashcode（）</span><br>
    &nbsp;&nbsp;&nbsp;&nbsp;<span style='font-size: 10px'>2.instance of是什么，作用是那些</span></p>

