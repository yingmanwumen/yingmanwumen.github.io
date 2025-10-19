+++
title = "(Draft) Data Science[1]: Linear Regression"
date = 2025-10-19

[taxonomies]
categories = ["data science"]
tags = ["self-teaching", "talk with chatgpt", "linear regression"]

[extra]
toc = true
+++

TODO

<!-- more -->

作为一个在此之前对 Data Science 一无所知、大学的统计考了60分刚刚好的 Coder，我需要以一种适合自己的方式去接触这门对我而言全新的学科。
线性回归模型似乎是 Hello World 级别的课题，就从这里开始吧。

我需要在正式开始之前，明确我想学到的知识是什么。作为小镇做题家，解题是我最擅长的事情了。下面这些问题是我认为至关重要的：

1. Linear Regression **是什么**、**有什么用**？
2. Linear Regression的 **优势**、**劣势** 分别是什么？
3. Linear Regression 的 **使用场景** 是什么？什么时候应该使用 Linear Regression、什么时候不应该使用 Linear Regression？
4. 有哪些衍生自 Linear Regression 的 model？
5. 使用 Linear Regression 的过程中、需要注意哪些问题？
6. 有什么现成的 library、framework、或者 resolver 可以用于使用 Linear Regression 来解决问题？
7. 如果我想深入学习 Linear Regression，有哪些经典的书籍、paper 可以作为 reference？

> 在 LLM 发明前，我需要阅读大量的书籍、提取并总结大量信息，运气好的话才有可能从中找到一部分答案 —— 更重要的是，我需要花费大量的时间才能解决我的问题。而现在有了 LLM，一切就变得简单了起来，只要有一套高效的、适合自己的学习体系，配合 LLM 广阔的知识，任何人都能在极短的时间内入门某个课题。

# Linear Regression 的定义及应用场景

## Linear

$$
\hat{y} = \beta_0 + \sum_j\beta_j\Phi_j(x_j) + \varepsilon
$$

线性模型中线性的意思是：只要参数是线性的，那么这个模型就是线性的。

上述公式中，即使 $\Phi(x)$ 是 $x^2$ 或者 $e^x$，都不影响这个模型是一个线性模型 —— 因为它的参数是线性的。

以下是非线性模型的例子：

$$
\begin{split}
&Y = \beta_1 x^{\beta2} \\\\
&Y = \beta_1 + \beta_2 e^{\beta_3x} \\\\
&Y = x_1 x_2
\end{split}
$$

一种直观的判断方法是观察 $x$ 是否只被一个 $\beta$ 影响。

## Regression

回归分析是一种研究数据的方法，目的是分析多个变量之间的相关性，以此来预测目标变量的值。

> “回归”一词最早由法兰西斯·高尔顿（Francis Galton）所使用。他曾对亲子间的身高做研究，发现父母的身高虽然会遗传给子女，但子女的身高却有逐渐“回归到中等（即人的平均值）”的现象。不过现在的回归已经和当初的意义不尽相同。 

$$
E(X|Y) = f(X, \beta)
$$

其中
- $X$ 为自变量
- $Y$ 为因变量
- $\beta$ 为回归系数
- $f$ 为回归函数
- $E$ 为期望

## 应用场景

什么情况下能使用 Linear Regression？

> **古典假设**
> - 样本是在总体中 **随机抽取** 的 
> - 因变量 $Y$ 在实直线上连续
> - 残差项独立随机且服从正态分布


# References

- [https://zh.wikipedia.org/zh-sg/%E8%BF%B4%E6%AD%B8%E5%88%86%E6%9E%90](https://zh.wikipedia.org/zh-sg/%E8%BF%B4%E6%AD%B8%E5%88%86%E6%9E%90)
- [https://zh.wikipedia.org/zh-sg/%E7%B7%9A%E6%80%A7%E5%9B%9E%E6%AD%B8](https://zh.wikipedia.org/zh-sg/%E7%B7%9A%E6%80%A7%E5%9B%9E%E6%AD%B8)
- [https://zh.wikipedia.org/zh-sg/%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83](https://zh.wikipedia.org/zh-sg/%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83)
- [https://zh.d2l.ai/chapter_linear-networks/linear-regression.html](https://zh.d2l.ai/chapter_linear-networks/linear-regression.html)
- [https://zhuanlan.zhihu.com/p/74673610](https://zhuanlan.zhihu.com/p/74673610)
- [https://zhuanlan.zhihu.com/p/127972563](https://zhuanlan.zhihu.com/p/127972563)

