title: 不可变类的变量声明
date: 2016-05-24 15:01:07
tags:
- java
- 基础
categories:
- java
- 基础知识
---

## 不可变类变量的声明的两种方式对比

主要从两个角度进行对比，一种是包装类，一种是String类。

### 包装类
> 包装类中只有Integer类涉及到一些特殊的方法区缓存问题，Integer会缓存-128至127的数字到方法区，而通过Integer.valueOf()方法声明的该区间内的变量都只是将指针指到方法区的变量上。

    ```
    Integer intA = 11;
    Integer intB = new Integer(11);
    // return false
    System.out.println(intA == intB);
    // return true
    System.out.println(intA.equals(intB));

    Integer intC = 128;
    Integer intD = 128;
    // return false
    System.out.println(intC == intD);

    Integer intE = 127;
    Integer intF = 127;
    // return true
    System.out.println(intE == intF);
    ```
    
- intA变量的声明方式会被优化为Integer.valueOf(11)，所以intA的变量是指向方法区上127的指针；
- 而intB变量是在堆上新声明对象，故两个对象无法在==中返回true；
- 而equals判断的是两个变量中的value是否相等，并非对象是否指向一个；
- 由于Integer的缓存区间是在-128至127之间，故intE==intF返回true，而intC == intD返回false。

### String类
> String类在声明变量的时候和Integer包装类类似，String声明对象会产生两种效果，一种会创建到方法区中，一种会在堆上创建。

    ```
    String a = new String("a");
    String c = "a";
    String b = a.intern();
    // return false
    System.out.println(c == a);
    // return true
    System.out.println(c == b);
    // return false
    System.out.println(a == b);
    ```
    
- 变量a会在堆中创建对象，变量c会在方法区中创建，变量b指针指向，在方法区中，和变量a值一样的变量指针；
- equals方法只判断value值是否相等，不判断指针指向的是否为同一个对象，故equals方法中变量都是一样的。
    
### 其他包装类
> Double包装类是双精度数字的包装类，故没有缓存。