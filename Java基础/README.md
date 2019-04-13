# Java基础

## Java概述

Java是一种通用的计算机编程语言，具有并发、基于类、面向对象、少依赖性等特点。它旨在让开发人员“write
once, run 
anywhere，编写一次，随处运行”（WORA），这意味着编译的Java代码可以在支持Java的所有平台上运行，而无需重新编译。

例如，你在Linux上编写和编译Java程序，可以在
Windows，Mac或Linux计算机上直接运行，而不需要对源代码进行任何修改。WORA通过将Java程序编译成**字节码**。然后Java虚拟机（
Java Virtual Machine，JVM）在各个平台上运行编译好的字节码。

![这里写图片描述](res/image01.png)

## Java常见概念

当人们谈论Java时，他们经常会提到总体概念的几个不同部分。这是因为Java不仅仅是一种编程语言。对于一个初学者来说，所有这些不同的“含义”可能会令人困惑，所以这里将简要解释它们，以便你知道人们在谈论什么。与Java相关的最常见概念是：

- [Java语言](#Java语言)
- Java字节码
- Java虚拟机（JVM）
- Java API
- Java运行时环境（JRE）
- Java开发人员工具包（JDK）
- Java代码约定
- Java标准版（JSE）
- Java企业版（JEE）
- Java应用服务器
- Java Micro Edition（JME）
- Java小程序
- JavaFX的
- Java开发者社区

### Java语言
首先，Java是一种编程语言。这意味着存在一种Java语言规范，它明确地告诉哪些元素是Java语言本身的一部分。换句话说，Java语言能够做什么。
Java文件存储在后缀为的文件中.java 。然后使用Java编译器将这些文件编译为Java字节代码，然后使用Java虚拟机（JVM）执行字节代码。Java编译器和JVM是Java Development Kit的一部分。

### Java字节码
用Java语言编写的Java程序被编译成Java字节码，可以由Java虚拟机执行。Java字节码存储在二进制.class文件中。