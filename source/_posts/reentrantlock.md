title: 并发锁ReentrantLock的优缺点
date: 2016-05-26 20:50:04
tags:
- java
- 基础
- 并发包
- ReentrantLock
- 多线程
categories:
- java
- 基础知识
---

## ReentrantLock的优点
- ReentrantLock可以创建多个，对同一线程中的不同条件进行lock操作，而synchronized只能锁一个对象或者方法块。
- ReentrantLock可以控制线程得到锁的顺序，达到公平锁；同时也可以像synchronized一样非公平分配。
- ReentrantLock可以查看锁的状态，是否被锁上。
- ReentrantLock可以查看当前有多少线程在查看锁。

## ReentrantLock的缺点
- 需要引入新的包。
- 需要自己在finally中控制锁的释放。
- synchronized可以作为关键字在方法声明中使用，比ReentrantLock来说减少了嵌套。