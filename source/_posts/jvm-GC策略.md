---
title: jvm GC策略
tags:
  - jvm内存
categories:
  - jvm
date: 2019/4/22 15:41:04 
declare: true
---
GC (Garbage Collection)的基本原理：将内存中不再被使用的对象进行回收，GC中用于回收的方法称为收集器，由于GC需要消耗一些资源和时间，Java在对对象的生命周期特征进行分析后，按照新生代、旧生代的方式来对对象进行收集，以尽可能的缩短GC对应用造成的暂停。
（1）对新生代的对象的收集称为minor GC；
（2）对旧生代的对象的收集称为Full GC；
（3）程序中主动调用System.gc()强制执行的GC为Full GC。

**JVM里的GC(Garbage Collection)的算法有很多种，如标记清除收集器，压缩收集器，分代收集器等等**
<!--more-->
 现在比较常用的是分代收集，即将内存分为几个区域，将不同生命周期的对象放在不同区域里:young generation，tenured generation和permanet generation。
绝大部分的objec被分配在young generation(生命周期短)，并且大部分的object在这里die。当young generation满了之后，将引发minor collection(YGC)。在minor collection后存活的object会被移动到tenured generation(生命周期比较长)。最后，tenured generation满之后触发major collection。major collection（Full gc）会触发整个heap的回收，包括回收young generation。permanet generation区域比较稳定，主要存放classloader信息。
young generation有eden、2个survivor 区域组成。其中一个survivor区域一直是空的，是eden区域和另一个survivor区域在下一次copy collection后活着的objecy的目的地。object在survivo区域被复制直到转移到tenured区。
我们要尽量减少 Full gc 的次数(tenured generation 一般比较大,收集的时间较长,频繁的Full gc会导致应用的性能收到严重的影响)。
## 堆内存GC ##
JVM(采用分代回收的策略)，用较高的频率对年轻的对象(young generation)进行YGC，而对老对象(tenured generation)较少(tenured generation 满了后才进行)进行Full GC。这样就不需要每次GC都将内存中所有对象都检查一遍。
## 非堆内存不GC ##
GC不会在主程序运行期对PermGen Space进行清理，所以如果你的应用中有很多CLASS(特别是动态生成类，当然permgen space存放的内容不仅限于类)的话,就很可能出现PermGen Space错误。