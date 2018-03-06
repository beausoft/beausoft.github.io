---
title: 使用BigDecimal进行精确运算
date: 2018-03-06 22:59:07
categories: Java
tags: Java
---
首先我们先来看如下代码示例：

```java
public class Test_1 {
    public static void main(String[] args) {
        System.out.println(0.06+0.01);
        System.out.println(1.0-0.42);
        System.out.println(4.015*100);
        System.out.println(303.1/1000);
    }
}
```
运行结果如下：

    0.06999999999999999
    
    0.5800000000000001
    
    401.49999999999994
    
    0.30310000000000004

你认为你看错了，但结果却是是这样的。问题在哪里呢？原因在于我们的计算机是二进制的。浮点数没有办法是用二进制进行精确表示。我们的CPU表示浮点数由两个部分组成：指数和尾数，这样的表示方法一般都会失去一定的精确度，有些浮点数运算也会产生一定的误差。如：2.4的二进制表示并非就是精确的2.4。反而最为接近的二进制表示是 2.3999999999999999。浮点数的值实际上是由一个特定的数学公式计算得到的。

其实java的float只能用来进行科学计算或工程计算，在大多数的商业计算中，一般采用java.math.BigDecimal类来进行精确计算。

在使用BigDecimal类来进行计算的时候，主要分为以下步骤：
1. 用float或者double变量构建BigDecimal对象。

2. 通过调用BigDecimal的加，减，乘，除等相应的方法进行算术运算。

3. 把BigDecimal对象转换成float，double，int等类型。

一般来说，可以使用BigDecimal的构造方法或者静态方法的valueOf()方法把基本类型的变量构建成BigDecimal对象。

```java
//BigDecimal b1 = new BigDecimal(Double.toString(0.48));
BigDecimal b2 = BigDecimal.valueOf(0.48);
```

对于常用的加，减，乘，除，BigDecimal类提供了相应的成员方法。

```java
public BigDecimal add(BigDecimal value);  //加法
public BigDecimal subtract(BigDecimal value); //减法 
public BigDecimal multiply(BigDecimal value); //乘法
public BigDecimal divide(BigDecimal value); //除法
```

进行相应的计算后，我们可能需要将BigDecimal对象转换成相应的基本数据类型的变量，可以使用floatValue()，doubleValue()等方法。

***注意：***

这里有个陷阱，当你使用new BigDecimal(Double d) 时，得到的BigDecimal中的long值并不是精确，详情可查源码，可追溯到native long doubleToRawLongBits(double value)；
这时使用new BigDecimal(String s)得到的即准确值，用于计算不会出现误差

**BigDecimal对小数的处理：**

BigDecimal.setScale()方法用于格式化小数点：

setScale(1)表示保留一位小数，默认用四舍五入的方式

setScale(1,BigDecimal.ROUND_DOWN)直接删除多余的小数位，如2.35会变成2.3

setScale(1,BigDecimal.ROUND_UP)进位处理，2.35变成2.4

setScale(1,BigDecimal.ROUND_HALF_UP)四舍五入，2.35变成2.4

setScale(1,BigDecimal.ROUND_HALF_DOWN)四舍五入，2.35变成2.3，如果是5则向下舍

scale指的是小数位后的位数，比如123.456的scale为3； 

==scale是BigDecimal类中的方法：==

```java
BigDecimal b = new BigDecimal("123.456");
b.scale();//3
```

==roundingMode==是小数的保留模式。它们都是BigDecimal中的常量字段,有很多种，示例如：


```java
new BigDecimal(Double.parseDouble(succRate) / 24).setScale(2, BigDecimal.ROUND_HALF_UP).toString();
new BigDecimal(svcSuccData.get("succRate").toString()).setScale(2, BigDecimal.ROUND_HALF_UP).toString()
```


参考来源：

1. [使用BigDecimal进行精确运算](https://www.cnblogs.com/chenssy/archive/2012/09/09/2677279.html)
2. [BigDecimal.setScale 处理java小数点](http://blog.csdn.net/lmb55/article/details/50253429)