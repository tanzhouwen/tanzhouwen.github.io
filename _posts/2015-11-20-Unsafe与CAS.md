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
>简单讲一下这个类。Java无法直接访问底层操作系统，而是通过本地（native）方法来访问。不过尽管如此，JVM还是开了一个后门，JDK中有一个类Unsafe，它提供了硬件级别的原子操作。
