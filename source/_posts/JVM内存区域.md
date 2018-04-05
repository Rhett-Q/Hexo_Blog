---
title: JVM内存区域
date: 2018-04-05 15:12:03
tags: JVM
---

## 区域分类

JVM内存区域总体分为两类：Heap区和非Heap区。
Heap区分为： `Eden Space`(伊甸园)、`Survivor Space`(幸存者区)、`Tenured Gen`(老年代-养老区)。  
非Heap分为: `Perm Gen`(永久代，方法区)、`JVM Stack`(Java虚拟机栈)、`Local Method Stack`(本地方法栈)

## Eden Space
一般情况下，对象创建在JVM堆中的新生代`Eden Space`中，当`Eden Space`没有足够空间分配给对象时，JVM会发起一次`Minor GC(Young GC)`。

## Survivor Sapce
JVM中有两块Survivor空间，一块为`From Survivor`，一块为`To Survivor`。在每次为新生对象分配空间时，会使用Eden Space和From Survivor Space的内存区域。  
![](https://i.imgur.com/hz1v3BE.jpg)
发生Minor GC时，将Eden和From Survivor中还存活的对象一次性复制到`To Survivor`区域中,而`Eden`和`From Survivor`中的数据被清空，`From Survivor`和`To Survivor`的身份互换，始终保持一个Survivor是空的，`To Survivor`中存放每一次M`inor GC`存活下来的对象，且对象Age加1，当对象的Age到达规定年龄的临界条件，会将其放入`Tenured Gen`。 

## Tenured Gen

可以进入老年代的对象的有3中情况：
1. 大对象，指需要大量连续内存空间的Java对象。
2. 在`Survivor Space`中Age到达年龄临界值的对象。
3. 在进行`Minor GC`时，`To Survivor`没有足够的空间存放存活下来的对象。

当老年代的内存不足时,JVM会发起`Major GC(Full GC)`。

## Perm Gen
`Perm Gen`(Permanent Generation Space)指内存永久保存的区域，又称为永久代。各个线程共享的内存区域，用于存储被虚拟机加载的类信息、常量、静态变量。

## JVM Stack

虚拟机栈时每个线程私有的，生命周期与线程相同，描述Java方法执行的内存模型，每个方法执行的同时，会在虚拟机栈中创建一个对应的栈帧，用于存放局部变量操作数栈、动态链接、方法出口信息等。每一个方法执行的过程，对应着一个栈帧的入栈出栈。

## Local Method Stack

本地方法栈的作用于虚拟机栈的类似，区别在于本地方法栈为JVM加载使用到的`Native`方法。