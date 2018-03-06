---
title: Math各个函数
date: 2018-03-06 22:58:25
categories: Java
tags: Java
---
#### Math.E

常量，比任何其他值都更接近 e（即自然对数的底数）的 double 值。

#### Math.PI

常量，比任何其他值都更接近 pi（即圆的周长与直径之比）的 double 值。 

#### Math.random

返回带正号的 double 值，该值大于等于 0.0 且小于 1.0。返回值是一个伪随机选择的数，在该范围内（近似）均匀分布。

第一次调用该方法时，它将创建一个新的伪随机数生成器，与以下表达式完全相同

new java.util.Random

之后，新的伪随机数生成器可用于此方法的所有调用，但不能用于其他地方。

此方法是完全同步的，可允许多个线程使用而不出现错误。但是，如果许多线程需要以极高的速率生成伪随机数，那么这可能会减少每个线程对拥有自己伪随机数生成器的争用。

#### Math.max

返回两个 int 值中较大的一个。也就是说，结果为更接近 Integer.MAX_VALUE 值的参数。如果参数值相同，那么结果也是同一个值。

#### Math.min

返回两个 long 值中较大的一个。也就是说，结果为更接近 Long.MAX_VALUE 值的参数。如果参数值相同，那么结果也是同一个值。

#### Math.abs

返回 int 值的绝对值。如果参数为非负数，则返回该参数。如果参数为负数，则返回该参数的相反数。

注意，如果参数等于 Integer.MIN_VALUE 的值（即能够表示的最小负 int 值），那么结果与该值相同且为负。

```java
Math.abs(-10);//输出10  
```

#### Math.floor

*向下取整*

round 则是4舍5入的计算，round方法，它表示“四舍五入”，算法为Math.floor(x+0.5)，即将原来的数字加上0.5后再向下取整，所以，Math.round(11.5)的结果为12，Math.round(-11.5)的结果为-11。 

```java
Math.floor(2);//=2
Math.floor(2.1);//=2
Math.floor(-2.1);//=-3
Math.floor(-2.5);//=-3
Math.floor(-2.9);//=-3
```

#### Math.ceil

*向上取整*

返回最小的（最接近负无穷大） double 值，该值大于等于参数，并等于某个整数。特殊情况如下：

- 如果参数值已经等于某个整数，那么结果与该参数相同。
- 如果参数为 NaN、无穷大、正 0 或负 0，那么结果与参数相同。
- 如果参数值小于 0，但是大于 -1.0，那么结果为负 0。

注意， Math.ceil(x) 的值与 -Math.floor(-x) 的值完全相同。

返回最小（最接近负无穷大）浮点值，该值大于等于该参数，并等于某个整数。

```java
Math.ceil(2);//=2
Math.ceil(2.1);//=3
Math.ceil(2.5);//=3
Math.ceil(2.9);//=3
```


#### Math.rint

*返回其值最接近参数并且是整数的 double 值。*

返回double值最接近参数的值，并等于某个整数。如果两个double值跟整数都同样接近，结果是整数值是偶数。特殊情况：

如果参数值已经等于某个整数，那么结果跟参数一样。

如果参数为NaN或无穷大，正零或负零，那么结果和参数一样。


```java
Math.rint(1654.9874);//=1655.0
Math.rint(-9765.134);//=-9765.0
```

```java
Math.rint(2);//=2
Math.rint(2.1);//=2
Math.rint(-2.5);//=-2
Math.rint(2.5);//=2
Math.rint(2.9);//=3
Math.rint(-2.9);//=-3
Math.rint(-2.49);//=-2
Math.rint(-2.51);//=-3
```

#### Math.round

*特殊的四舍五入*

返回最接近参数的 long。结果将舍入为整数：加上 1/2，对结果调用 floor 并将所得结果强制转换为 long 类型。换句话说，结果等于以下表达式的值：

(long)Math.floor(a + 0.5d)

特殊情况如下：

- 如果参数为 NaN，那么结果为 0。
- 如果结果为负无穷大或任何小于等于 Long.MIN_VALUE 的值，那么结果等于 Long.MIN_VALUE 的值。
- 如果参数为正无穷大或任何大于等于 Long.MAX_VALUE 的值，那么结果等于 Long.MAX_VALUE 的值。

返回舍入为最接近的 long 值的参数值。

```java
Math.round(3.14);//3
Math.round(3.5);//4
Math.round(-3.14);//-3
Math.round(-3.5);//-3
```

还有一个问题就是==奇进偶舍==的原则

今天客户跑过来跟我说，我们程序里面计算的价格不对，我检查了一下，发现价格是经过折算后的价格，结果是可能小数位较多，而单据上只能打印两位价格，所以就对价格调用Math.Round(price,2)函数进行四舍五入。

而出现问题的单价就是1.805，函数Math.Round(1.805,2)的返回值却是1.80，按照四舍五入的原则，结果应该是1.81才对。

在一番google之后，发现微软是对了，是我们错了。

原来四舍五入也有个国际惯例，叫奇进偶舍，意思是当舍入位前面一位是奇数时，就进，为偶数时，就舍，这也是体现公平性的原理。

可是国际惯例往往在国内很多企业行不通，为了应付他们的要求，采用Math.Round(price,2,MidpointRounding.AwayFromZero)就可以了。

#### Math.copySign

*copySign(x,y) 返回 用y的符号取代x的符号后新的x值*

返回带有第二个浮点参数符号的第一个浮点参数。注意，与 StrictMath.copySign 方法不同，此方法不要求将 NaN sign 参数视为正值；允许实现将某些 NaN 参数视为正，将另一些视为负，以获得更好的性能。

参数：

- magnitude - 提供结果数值的参数
- sign - 提供结果符号的参数

返回：

一个值，带有 magnitude 的数值， sign 的符号。

```java
Math.copySign(-1.0, 2.0);//输出1.0  
Math.copySign(2.0, -1.0);//输出-2.0  
```

#### Math.nextAfter

*nextAfter(a,b) 返回(a,b)或(b,a)间与a相邻的浮点数 b可以比a小*

返回第一个参数和第二个参数之间与第一个参数相邻的浮点数。如果两个参数比较起来相等，则返回第二个参数。

特殊情况如下：

- 如果任一参数为 NaN，则返回 NaN。
- 如果两个参数都为有符号的 0，则不做更改地返回 direction（根据要求，如果参数比较起来相等，将返回第二个参数）。
- 如果 start 为 ±Double.MIN_VALUE，而 direction 的值要求结果为一个比 start 小的数值，那么将返回 0，并带有与 start 相同的符号。
- 如果 start 为无穷大，而 direction 的值要求结果为一个比 start 小的数值，则返回 Double.MAX_VALUE，并带有与 start 相同的符号。
- 如果 start 等于 ±Double.MAX_VALUE，而 direction 的值要求结果为一个比 start 大的数值，则返回无穷大，并带有与 start 相同的符号。

参数：

- start - 起始浮点值。
- direction - 一个值，指示应返回 start 的某个邻数还是 start。

返回：

start 和 direction 之间与 start 相邻的浮点数。

```java
Math.nextAfter(1.2, 2.7);//输出1.2000000000000002  
Math.nextAfter(1.2, -1);//输出1.1999999999999997 
```

*所以这里的b是控制条件*

#### Math.nextUp

*nextUp(a) 返回比a大一点点的浮点数*

```java
Math.nextUp(1.2);//输出1.2000000000000002
```

返回 d 和正无穷大之间与 d 相邻的浮点值。此方法在语义上等同于 nextAfter(d, Double.POSITIVE_INFINITY)；但是， nextUp 实现的返回速度可能比其等价 nextAfter 调用快。

特殊情况如下：

- 如果参数为 NaN，那么结果为 NaN。
- 如果参数为正无穷大，那么结果为正无穷大。
- 如果参数为 0，那么结果为 Double.MIN_VALUE。

参数：

d - 起始浮点值。

返回：

离正无穷大较近的相邻浮点值。

#### Math.nextDown

*nextDown(a) 返回比a小一点点的浮点数*

```java
Math.nextDown(1.2);//输出1.1999999999999997
```


### 三角函数与反三角函数

cos求余弦

sin求正弦

tan求正切

acos求反余弦

asin求反正弦

atan求反正切

atan2(y,x)求向量(x,y)与x轴夹角

```java
Math.acos(-1.0);//输出圆周率3.14...  
Math.atan2(1.0, 1.0);//输出 π/4 的小数值 
```

### 开根号

cbrt(x)开立方 

sqrt(x)开平方 

hypot(x,y)求sqrt(x\*x+y\*y)在求两点间距离时有用sqrt((x1-x2)^2+(y1-y2)^2)

```java
Math.sqrt(4.0);//输出2.0  
Math.cbrt(8.0);//输出2.0  
Math.hypot(3.0, 4.0);//输出5.0 
```

### 对数

log(a) a的自然对数(底数是e)

log10(a) a 的底数为10的对数

log1p(a) a+1的自然对数

值得注意的是，前面其他函数都有重载，对数运算的函数只能传double型数据并返回double型数据


```java
Math.log(Math.E);//输出1.0  
Math.log10(10);//输出1.0  
Math.log1p(Math.E-1.0);//输出1.0 
```

### 幂

exp(x) 返回e^x的值

expm1(x) 返回e^x - 1的值

pow(x,y) 返回x^y的值

这里可用的数据类型也只有double

```java
Math.exp(2);//输出E^2的值  
Math.pow(2.0, 3.0);//输出8.0  
```

### 转换

toDegrees(a) 弧度换角度 

toRadians(a) 角度换弧度

```java
Math.toDegrees(Math.PI);//输出180.0  
Math.toRadians(180);//输出 π 的值 
```
