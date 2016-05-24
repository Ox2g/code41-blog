title: java中String、StringBuffer和StringBuilder的区别
date: 2016-05-24 11:12:44
tags:
- java
- 基础
- String
categories:
- java
- 基础知识
---

1. 可变和不可变
  - String类中使用字符数组保存字符串，被final修饰，所以String对象是不可变类。
    ```
    private final char value[];
    ```
  - StringBuilder和StringBuffer都是继承于AbstractStringBuilder类，也是通过使用字符数组来保存字符串，但是没有final修饰，故可以通过append方法进行字符串的拼接操作。
    ```
    char[] value;
    ```

2. 是否线程安全
  - String中对象是不可变的，所以可以理解为常量，是 **线程安全的**
  - AbstractStringBuilder是StringBuilder与StringBuffer的公共父类，定义了一些字符串的基本操作，如expandCapacity、append、insert、indexOf等公共方法。__StringBuffer对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。__
    ```
    public synchronized StringBuffer append(CharSequence s, int start, int end)
    {
        super.append(s, start, end);
        return this;
    }
    
    public synchronized StringBuffer reverse() {
        super.reverse();
        return this;
    }
    ```
  - StringBuilder并没有对方法进行加同步锁，所以是非线程安全的。

3. StringBuffer和StringBuilder的共同点
  - StringBuilder与StringBuffer有公共父类AbstractStringBuilder(抽象类)。
  - StringBuilder和StringBuffer的方法都会调用AbstractStringBuilder中的公共方法。
  - StringBuffer的方法会使用synchronized关键字，保证同步。
  - 程序如果不是多线程，使用StringBuilder效率会更高。
  
> JDK6之后，String的拼接会自动优化为StringBuilder。
  