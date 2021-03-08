---
title: Number theory 
categories: [acm]
comments: true
---
# 数论简介

主要研究整数的性质

按研究方法来看，数论大致可分为`初等数论和高等数论`。初等数论是用初等方法研究的数论，它的研究方法本质上说，就是利用整数环的整除性质，主要包括`整除理论、同余理论、连分数理论`。高等数论则包括了更为深刻的数学研究工具。它大致包括`代数数论、解析数论、计算数论`等等。

## 门类

**初等数论**

初等数论主要就是研究整数环的整除理论及同余理论。此外它也包括了连分数理论和少许不定方程的问题。本质上说，初等数论的研究手段局限在整除性质上。

初等数论中经典的结论包括`算术基本定理`、欧几里得的`质数无限证明`、`中国剩余定理`、`欧拉定理`（其特例是费马小定理）、高斯的`二次互反律`， 勾股方程的`商高定理`、佩尔方程的`连分数求解法`等等。

**解析数论**

借助微积分及复分析（即复变函数）来研究关于整数的问题，主要又可以分为**乘性数论与加性数论**两类。乘性数论藉由研究积性生成函数的性质来探讨素数分布的问题，其中`质数定理`与`狄利克雷定理`为这个领域中最著名的古典成果。加性数论则是研究整数的加法分解之可能性与表示的问题，`华林问题`是该领域最著名的课题。

解析数论的创立当归功于黎曼。他发现了黎曼zeta函数之解析性质与数论中的素数分布问题存在深刻联系。确切的说， 黎曼ζ函数的非平凡零点的分布情况决定了素数的很多性质。黎曼猜测， 那些零点都落在复平面上实部为1/2的直线上。这就是著名的黎曼假设—千禧年大奖难题之一。值得注意的是， 欧拉实际上在处理素数无限问题时也用到了解析方法。

解析数论方法除了`圆法`、`筛法`等等之外， 也包括`和椭圆曲线相关的模形式理论`等等。此后又发展到自守形式理论，从而和表示论联系起来。

**代数数论**

代数数论，将整数环的数论性质研究扩展到了更一般的整环上，特别是代数数域。一个主要课题就是关于代数整数的研究，目标是为了更一般地解决不定方程求解的问题。其中一个主要的历史动力来自于寻找费马大定理的证明。

代数数论更倾向于从代数结构角度去研究各类整环的性质， 比如在给定整环上是否存在算术基本定理等等。

这个领域与代数几何之间的关联尤其紧密， 它实际上也构成了交换代数理论的一部分。它也包括了其他深刻内容，比如`表示论`、`p-adic理论`等等。

**几何数论**

主要在于通过几何观点研究整数（在此即格点， 也称整点）的分布情形。最著名的定理为`Minkowski定理`。这门理论也是有闵科夫斯基所创。对于研究二次型理论有着重要作用。 

**计算数论**

借助电脑的算法帮助研究数论的问题，例如素数测试和因数分解等和`密码学`息息相关的课题。

**超越数论**

研究数的超越性，其中对于`欧拉常数`与`特定的riemann ζ函数值`之研究尤其令人感到兴趣。此外它也探讨了数的`丢番图逼近理论`。

**组合数论**

利用组合和机率的技巧，非构造性地证明某些无法用初等方式处理的复杂结论。这是由保罗·艾狄胥开创的思路。比如`兰伯特猜想的简化证明`。

**算术代数几何**

这是数论发展到目前为止最深刻最前沿的领域， 可谓集大成者。它从代数几何的观点出发，通过深刻的数学工具去研究数论的性质。比如怀尔斯证明费马猜想就是这方面的经典实例。整个证明几乎用到了当时所有最深刻的理论工具。

当代数论的一个重要的研究指导纲领，就是著名的郎兰兹纲领。

## 猜想
* 哥德巴赫猜想：是否每个大于2的偶数都可写成两个质数之和？
* 孪生素数猜想：孪生素数就是差为2的素数对，例如11和13。是否存在无穷多的孪生素数？
* 斐波那契数列内是否存在无穷多的素数？
* 是否存在无穷多的梅森素数？（指形如2p－1的正整数，其中指数p是素数，常记为Mp 。若Mp是素数，则称为梅森素数）
* 1995年怀尔斯和理查·泰勒证明了历时350年的费马猜想（费马大定理）
* 黎曼猜想

![d046977e9b63e790bd8fe0eb3913a949.png](en-resource://database/775:1)


# acm数论基础

[ACM数论总结 · 简](https://blog.csdn.net/xieshimao/article/details/6425099)

[ACM-数论完全总结（知识点+模板）](https://blog.csdn.net/weixin_43093481/article/details/82229718)