《七周七语言》
=======================

## 作者
     [美] Bruce A·Tate 
  
## 简介
> 从计算机发展史早期的Cobol、Fortran到后来的C、Java，编程语言的家族不断壮大。除了这些广为人知的语言外，还涌现了Erlang、Ruby等后起之秀，它们虽被喻为小众语言，但因其独特性也吸引了为数不少的追随者。
Bruce A. Tate是软件行业的一名老兵，他有一个宏伟目标：用一本书的篇幅切中要害地探索七种不同的语言。本书就是他的成果。书中介绍了Ruby、Io、Prolog、Scala、Erlang、Clojure和Haskell这七种语言，关注每一门语言的精髓和特性，重点解决如下问题：这门语言的类型模型是什么，编程范式是什么，如何与其交互，有哪些决策构造和核心数据结构，有哪些独特的核心特性。
在这个飞速发展的信息时代，程序员仅仅掌握甚至精通一门语言是远远不够的。了解多门语言蕴涵的思维方式，在编码中互相借鉴，再挑出一两门对自己口味的语言深入学习，这些已经成为在软件行业中安身立命之本。从这个意义上说，每个程序员都应该看看这本《七周七语言》。

## 作者简介
> Bruce A. Tate RapidRed公司总裁，该公司主要为Ruby轻量级开发提供咨询。他曾任职于IBM公司，并担任过多家公司的客户解决方案总监和CTO。著作有十余本，包括荣获Jolt大奖的Better, Faster, Lighter Java。


## 豆瓣链接
[豆瓣链接](https://book.douban.com/subject/10555435/)

## 目录

## 内容

##### Io
* Io是一种原型语言；不区分类和对象，可以通过复制现有对象创建新对象；所有事情都是对象；所有与对象的交互都是消息；不是实例化类，而是复制原型
  - Vehicle:=Object clone
  - Vehicle descp:='something'  //类似散列表
  - Vehicle descp // 调用
  - Vehicle slotNames ==> ('list', 'descp')
  - Vehicle type
  - Car:=Vehicle clone  // 类型以大写开头
  - farrari:Vehicle clone  // 对象
  - Car drive:=method('vroom') // 方法是对象，可以赋值给一个slot
  - Lobby是主命名空间，包含所有已命名对象
* list和map
  - list(1,2,3,4) sum
  - list(1) append(2)
  - list(1,2) pop
  - list() isEmpty
* 0是true；true, false, nil都是单例
