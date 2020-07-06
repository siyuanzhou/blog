---
layout: post
title: "MapReduce原理介绍"
date: 2019-10-27 10:36
toc: true
comments: true
categories: 技术学习
tags: 
	- 分布式
typora-root-url: ./

---

MapReduce 是一个编程模型，也是一个处理和生成超大数据集的算法模型的相关实现。用户首先创建一个 Map 函数处理一个基于 key/value pair 的数据集合，输出中间的基于 key/value pair 的数据集合；然后再创建一个 Reduce 函数用来合并所有的具有相同中间 key 值的中间 value 值。

<!--more-->

![1571990929156](/../assets/pic/2019-10-27-MapReduce%E5%8E%9F%E7%90%86%E4%BB%8B%E7%BB%8D/1571990929156.png)

![1571991040356](/../assets/pic/2019-10-27-MapReduce%E5%8E%9F%E7%90%86%E4%BB%8B%E7%BB%8D/1571991040356.png)

![1571991244030](/../assets/pic/2019-10-27-MapReduce%E5%8E%9F%E7%90%86%E4%BB%8B%E7%BB%8D/1571991244030.png)

##### 关系代数

关系代数是一种过程化查询语言。它包括一个运算的集合，这些运算以一个或两个关系为输入，产生一个新的关系作为结果。

五个基本操作：
  集合并(∪)、集合差(-)、笛卡尔积(×)、选择(σ)、投影(π)、更名(ρ)

四个组合操作：
  集合交(∩)、联接(等值联接)、自然联接(RS ⋈ )、除法(÷) 、赋值（←）



![img](/../assets/pic/2019-10-27-MapReduce%E5%8E%9F%E7%90%86%E4%BB%8B%E7%BB%8D/20151030095843101.png)

通过将Map调用的输入数据自动分割为M个数据片段的集合，Map调用被分布到多台机器上执行。输入的数据片段能够在不同的机器上并行处理。使用分区函数将Map调用产生的中间key值分成R个不同分区（例如，hash(key) mod R），Reduce调用也被分布到多台机器上执行。分区数量（R）和分区函数由用户来指定。

1.用户程序调用的MapReduce库首先将输入文件分成M个数据片度，每个数据片段的大小一般从 16MB到64MB(可以通过可选的参数来控制每个数据片段的大小)。然后master在机群中创建大量的用户程序副本（It then starts up many copies of the program on a cluster of machines，即，把map/reduce函数给不同的机器执行）。
2.这些程序副本中的有一个特殊的程序–master。副本中其它的程序都是worker程序，由master分配任务。有M个Map任务和R个Reduce任务将被分配，master将一个Map任务或Reduce任务分配给一个空闲的worker。
3.被分配了map任务的worker程序读取相关的输入数据片段，从输入的数据片段中解析出key/value pair，然后把key/value pair传递给用户自定义的Map函数，由Map函数生成并输出的中间key/value pair，并缓存在本机内存中。
4.缓存中的key/value pair通过分区函数分成R个区域，之后周期性的写入到本地磁盘上。缓存的key/value pair在本地磁盘上的存储位置将被回传给master，由master负责把这些存储位置再传送给Reduce worker（图中没画出来）。
5.当Reduce worker程序接收到master程序发来的数据存储位置信息后，Reduce worker使用remote procedure calls从Map worker所在主机的磁盘上读取这些缓存数据。当Reduce worker读取了（他所需要的）所有的中间数据后，通过对key进行排序后使得具有相同key值的数据聚合在一起。由于许多不同的key值会映射到相同的Reduce任务上，因此必须进行排序（从而保证相同的key的序列依次出现）。如果中间数据太大无法在内存中完成排序，那么就要在外部进行排序。
6.Reduce worker程序遍历排序后的中间数据，对于每一个唯一的中间key值，Reduce worker程序将这个key值和它相关的中间value值的集合传递给用户自定义的Reduce函数并计算出最终输出。Reduce函数的输出被追加到所属分区的输出文件。
7.当所有的Map和Reduce任务都完成之后，master唤醒用户程序。在这个时候，在用户程序里的对MapReduce调用才返回。

在成功完成任务之后，MapReduce的输出存放在R个输出文件中（对应每个Reduce任务产生一个输出文件，文件名由用户指定）。一般情况下，用户不需要将这R个输出文件合并成一个文件–他们经常把这些文件作为另外一个MapReduce的输入，或者在另外一个可以处理多个分割文件的分布式应用中使用

