---
layout:     post   				    # 使用的布局（不需要改）
title:      Unsafe与CAS 		    # 标题 
date:       2015-11-20 				# 时间
author:     Tandy 					# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - java基础
---

## Unsafe与CAS

### Unsafe
简单讲一下这个类。Java无法直接访问底层操作系统，而是通过本地（native）方法来访问。不过尽管如此，JVM还是开了一个后门，JDK中有一个类Unsafe，它提供了硬件级别的原子操作。

>这个类尽管里面的方法都是public的，但是并没有办法使用它们，JDK API文档也没有提供任何关于这个类的方法的解释。总而言之，对于Unsafe类的使用都是受限制的，只有授信的代码才能获得该类的实例，当然JDK库里面的类是可以随意使用的。

>从第一行的描述可以了解到Unsafe提供了硬件级别的操作，比如说获取某个属性在内存中的位置，比如说修改对象的字段值，即使它是私有的。不过Java本身就是为了屏蔽底层的差异，对于一般的开发而言也很少会有这样的需求。

>举两个例子，比方说：
>`public native long staticFieldOffset(Field paramField);`

>这个方法可以用来获取给定的paramField的内存地址偏移量，这个值对于给定的field是唯一的且是固定不变的。再比如说：
>`public native int arrayBaseOffset(Class paramClass);
public native int arrayIndexScale(Class paramClass);`

>前一个方法是用来获取数组第一个元素的偏移地址，后一个方法是用来获取数组的转换因子即数组中元素的增量地址的。最后看三个方法：
>`public native long allocateMemory(long paramLong);
public native long reallocateMemory(long paramLong1, long paramLong2);
public native void freeMemory(long paramLong);`

>分别用来分配内存，扩充内存和释放内存的。

>当然这需要有一定的C/C++基础，对内存分配有一定的了解，这也是为什么我一直认为C/C++开发者转行做Java会有优势的原因。
