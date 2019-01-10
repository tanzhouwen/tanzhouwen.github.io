---
layout:     post
title: JVM的4种垃圾回收算法、垃圾回收机制与总结
date: 2018-11-01
author: Tandy
header-img: img/post-bg-2015.jpg
catalog: true
tags: JVM
grammar_code: true
---
## JVM的4种垃圾回收算法、垃圾回收机制与总结
### 垃圾回收算法
 #### 1. 标记清除
 
标记-清除算法将垃圾回收分为两个阶段：**标记阶段和清除阶段**。

在标记阶段首先通过**根节点(GC Roots)**，标记所有从根节点开始的对象，未被标记的对象就是未被引用的垃圾对象。然后，在清除阶段，清除所有未被标记的对象。

![](https://raw.githubusercontent.com/tanzhouwen/tanzhouwen.github.io/master/images/jvm-gc-marker-clear.jpg)

##### 适用场合：

 - 存活对象较多的情况下比较高效
 - 适用于年老代（即旧生代）
##### 缺点：

 - 容易产生内存碎片，再来一个比较大的对象时（典型情况：该对象的大小大于空闲表中的每一块儿大小但是小于其中两块儿的和），会提前触发垃圾回收
 - 扫描了整个空间两次（第一次：标记存活对象；第二次：清除没有标记的对象）

#### 2. 复制算法

从根节点(GC Roots)进行扫描，标记出所有的存活对象，并将这些存活的对象复制到一块儿新的内存（图中下边的那一块儿内存）上去，之后将原来的
那一块儿内存（图中上边的那一块儿内存）全部回收掉，**现在的商业虚拟机都采用这种收集算法来回收新生代**。

![](https://raw.githubusercontent.com/tanzhouwen/tanzhouwen.github.io/master/images/jvm-gc-copy.jpg)

##### 适用场合：

 - 存活对象较少的情况下比较高效
 - 扫描了整个空间一次（标记存活对象并复制移动）
 - 适用于年轻代（即新生代）：基本上98%的对象是”朝生夕死”的，存活下来的会很少
##### 缺点：

 - 需要一块儿空的内存空间
 - 需要复制移动对象
 
#### 3. 标记整理

复制算法的高效性是建立在存活对象少、垃圾对象多的前提下的。

这种情况在新生代经常发生，但是在老年代更常见的情况是大部分对象都是存活对象。如果依然使用复制算法，由于存活的对象较多，复制的成本也将很高。

![](https://raw.githubusercontent.com/tanzhouwen/tanzhouwen.github.io/master/images/jvm-gc-marker-sort.jpg)

标记-压缩算法是一种老年代的回收算法，它在标记-清除算法的基础上做了一些优化。

首先也需要从根节点开始对所有可达对象做一次标记，但之后，它并不简单地清理未标记的对象，而是将所有的存活对象压缩到内存的一端。之后，清理边界外所有的空间。这种方法既避免了碎片的产生，又不需要两块相同的内存空间，因此，其性价比比较高。

#### 4. 分代收集算法

**分代收集算法**就是目前虚拟机使用的回收算法，它解决了标记整理不适用于老年代的问题，将内存分为各个年代。一般情况下将堆区划分为老年代（Tenured Generation）和新生代（Young Generation），在堆区之外还有一个代就是永久代（Permanet Generation）。

在不同年代使用不同的算法，从而使用最合适的算法，新生代存活率低，可以使用复制算法。而老年代对象存活率搞，没有额外空间对它进行分配担保，所以只能使用标记清除或者标记整理算法。
![](https://raw.githubusercontent.com/tanzhouwen/tanzhouwen.github.io/master/images/jvm-gc-generational-collection.jpg)
