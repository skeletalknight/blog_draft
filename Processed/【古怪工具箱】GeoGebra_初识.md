---
title: 【古怪工具箱】GeoGebra_初识
keywords: 工具箱、观星篇
categories:
- [工具箱]
- [观星篇]
cover: [post_content/【古怪工具箱】GeoGebra_初识/R.jpg]
banner:
  bannerText: Draw Math
toc: true
single_column: true
author: 尼尔波波
mathjax: true
---

## 函数序列和

在高等数学中，我们可能会遇到以序列和表达的函数形式，例如：
$$
f(x)=\sum_{n=0}^{m}a_nx^n \qquad \newline or \newline \qquad f(x)=\frac{a_0}{2}+\sum_{n=0}^{m}\left( a_n\cos nx+b_n\sin nx \right)
$$
想要在GeoGebra中输入函数序列和的形式，我们需要调用Sum和Sequence两个指令。

- **Sum**指令可以对指定序列加和，例如：
	-   `Sum({1, 2, 3})` 产生数值 *a = 6*。
	-   `Sum({x^2, x^3})` 产生 $f(x) = x^2 + x^3$。
	-   `Sum({(1, 2), (2, 3)})` 产生点 *A = (3, 5)*。
	-   `Sum({(1, 2), 3})` 产生点 *B = (4, 5)*。
- **Sequence**指令可以产生对应的序列，其语法为：`Sequence(函数形式，变量名，起始点，终点)`，例如：
  -  `Sequence(i,i,1,4)`产生{1，2，3，4}
  -  `Sequence(x^i,i,1,3)`产生{$x，x^2，x^3$}

综上，结合Sum和Sequence指令，我们可以在GeoGebra里方便地表示序列和。

例如，对于y=x的正弦级数：
$$
f(x)=2\sum_{n=0}^{m}\left(\frac{(-1)^{n+1}}{n}\sin nx \right)
$$
可以写作`Sum(Sequence(((2)/(n)) (-1)^(n+1) sin(n x),n,1,m))`

其中m是加和的数量，我们可以另起滑块对其进行修改。

<img src="/post_content/【古怪工具箱】GeoGebra_初识/Snipaste_2024-03-23_10-35-02.png" alt="Snipaste_2024-03-23_10-35-02" style="zoom:50%;" />

于是我们就可以直观的看到，傅里叶级数如何逼近原函数了。

以下是笔者对其他两个函数的模拟动图：

<img src="/post_content/【古怪工具箱】GeoGebra_初识/2024-03-23 00-06-161.gif" alt="2024-03-23 00-06-16~1" style="zoom:100%;" />

<img src="/post_content/【古怪工具箱】GeoGebra_初识/格式工厂2024-03-23 09-21-56.gif" alt="格式工厂2024-03-23 09-21-56" style="zoom:100%;" />

对于Sum和Sequence的更多应用，可以参考官网：[Sum 指令 - GeoGebra 手册](https://wiki.geogebra.org/zh/Sum_指令)
