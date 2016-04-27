title: JVM虚拟机GC回收算法
date: 2016-04-27 15:12:34
tags:
- java
- JVM
- 基础
categories:
- java
- 基础知识
---

## JVM垃圾收集算法 

> 由于大多数虚拟机实现的细节不同，主要有4种垃圾收集算法：标记-清除算法、复制算法、标记-整理算法和分代收集算法。其中标记-清除为基础算法，复制算法和标记-整理算法都是基于该算法衍生而出，分代收集算法则是之前3种算法的整合。

### 标记-清除算法[Mark-Sweep]

示意图如下：
![mark-sweep](jvm-gc-algorithm/mark-sweep.png)

### 复制算法[Copying]

示意图如下：
![mark-sweep](jvm-gc-algorithm/copying.png)

### 标记-整理算法[Mark-Compact]

示意图如下：
![mark-sweep](jvm-gc-algorithm/mark-compact.png)

### 分代收集算法[Generational Collection]

根据存活周期不同将内存划分多块，然后针对不同不同周期（年代）对象使用不同的，最应当的垃圾回收算法。
> java堆中划分为新生代和老年代，针对新生代(每次垃圾回收，有大批对象死亡，)使用Copying算法，而针对老年代使用Mark-Compact算法。

