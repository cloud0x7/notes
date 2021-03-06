《Java核心技术·卷1：基础知识》
=======================

## 作者
   （美）Cay S. Horstmann / （美）Gary Cornell  
  
## 简介
> Java领域最有影响力和价值的著作之一，拥有20多年教学与研究经验的资深Java技术专家撰写（获Jolt大奖），与《Java编程思想》齐名，10余年全球畅销不衰，广受好评。第9版根据JavaSE7全面更新，同时修正了第8版中的不足，系统全面讲解Java语言的核心概念、语法、重要特性和开发方法，包含大量案例，实践性强。

《Java核心技术·卷1：基础知识》共14章。第1章概述了Java语言与其他程序设计语言不同的性能；第2章讲解了如何下载和安装JDK及本书的程序示例；第3章介绍了变量、循环和简单的函数；第4章讲解了类和封装；第5章介绍了继承；第6章解释了接口和内部类；第7章概述了图形用户界面程序设计知识；第8章讨论AWT的事件模型；第9章探讨了SwingGUI工具箱；第10章讲解如何部署自己的应用程序或applet；第11章讨论异常处理；第12章概要介绍泛型程序设计；第13章讲解Java平台的集合框架；第14章介绍了多线程。本书最后还有一个附录，其中列出了Java语言的保留字。

## 作者简介
> Cay S. Horstmann，圣何塞州立大学计算机科学系教授、Java语言的倡导者，也是《Scala for the Impatient》一书（Addison-Wesley，2012）的作者和《Core JavaServer· Faces，3rd》一书（Prentice Hall, 2010）的合著者。他还经常在计算机会议上发表演讲。
Cray Cornell，已经教授程序设计专业课程20多年，并撰写了多部专著。他是Apress的创始人之一，他写的程序设计专业书籍非常畅销，曾荣获Jolt震撼大奖，并获得Visual Basic Magazine的读者最喜爱作品大奖。

## 豆瓣链接
[豆瓣链接](https://book.douban.com/subject/25762168/)

## 目录

## 内容

#### 第3章 Java基本的程序设计结构
* 数据类型
  - 8种基本类型：4种整型、2种浮点类型、1种字符型和1种boolean类型
    - byte、short、int、long / float、double / char / boolean
  - 整型值和布尔值宰不能相互转换
  - final声明常量
  - 枚举类型： enum Size {SMALL, MEDIUM, LARGE}
  - 字符串：不能修改字符串中的字符，但可以修改变量。优点：可共享公共的字符串存储池；
    - '+'或substring操作产生的结果并不是共享的，不能使用'=='检测相等，所以__不要使用==检测相等__。使用s.equals(t)
    - 使用StringBuilder进行大量的字符串拼接
  - 流程控制
    - switch：case标签必须是整数或枚举常量，不能测试字符串
  - 大数值：java.math中的BigInterger、BigDecimal，使用add, multiply等方法
  - Java没有运算符重载功能，除了“+”
  - 数组：一旦创建就不能再改变它的大小
  
#### 第4章 对象与类
* 需要返回一个可变对象，首先对它进行克隆:testDay.clone();