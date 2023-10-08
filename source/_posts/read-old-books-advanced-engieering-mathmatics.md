---
title: 旧书新读-《高等工程数学》
date: 2022-01-20 10:02:08
mathjax:
  enable: true
tags: books
---

- [线性空间](#线性空间)
  - [线性组合/线性表出](#线性组合线性表出)
  - [线性相关](#线性相关)
  - [唯一线性表出定理](#唯一线性表出定理)
  - [基](#基)
  - [坐标](#坐标)
  - [维度](#维度)
  - [基与坐标变换](#基与坐标变换)
  - [欧式空间](#欧式空间)
  - [子空间](#子空间)

> 线性代数应该怎么学? (ref: https://www.zhihu.com/question/311724817)

> 要学好线性代数，最重要的是抓住线性代数的主线。线性代数的主线就是线性空间以及线性映射，整个线性代数的概念公式定义定理都是围绕着线性空间以及线性映射展开的。你要做的，就是紧紧抓住这条主线，把线性代数的所有知识点串联起来，然后融会贯通，自然就能学好线性代数了。

> 1，线性空间。线性空间的定义比较抽象，简单的说，就是向量组成的一个集合，这个集合可以定义加法以及纯量乘法，并且对加法以及乘法满足交换律结合律以及分配率。这个集合以及定义在集合上的代数运算就是线性空间。

> 研究线性空间有几个途径，一是基与维数，二是同构，三是子空间与直和以及商空间，四是线性映射。

> 先讲讲基与维数。一个线性空间必定存在基，线性空间的任意元素都可以由基线性表出，且表出方式唯一，这个唯一的表出的组合就是这个元素在这个基下的坐标。线性表出且表出方式唯一的充分必要条件是什么？这里又引出了线性无关以及极大线性无关组的概念，极大线性无关组元素的个数又能引出秩的概念。由秩又能引出维度的概念。以上这些概念都是为了刻画线性空间的基与维数而衍生出来的，并不是凭空出现无中生有的。

> 下面再谈谈同构。线性空间千千万，应如何研究呢？同构就是这样一个强大的概念，任何维数相同的线性空间之间是同构的，空间的维数是简单而深刻的，简单的自然数居然能够刻画空间最本质的性质。借助于同构，要研究任意一个n维线性空间，只要研究Rⁿ就行了。

> n维线性空间作为一个整体，我们自然想到能不能先研究它的局部性质？所以自然而然的导出了子空间的概念以及整个空间的直和分解。直和分解要求把整个空间分解为两两不交的子空间之和，通过研究各个简单的子空间的性质，从而得出整个空间的性质。

> 2，线性映射。

> 前面讲了线性空间，舞台搭好了，轮到主角：线性映射登场了。

> 线性映射的定义这里就不赘述了。我们小学就学过正比例函数y=kx，这是一个最简单的一维线性映射，也是一个具体的线性映射'模型'，线性映射的所有性质对比着正比例函数来看，一切都是那么简单易懂。现在把定义域从一维升级到多维，值域也从一维升级到多维，然后正比例系数k也升级为一个矩阵，那么这个正比例函数就升级为一个线性映射了。

> 1)线性映射的核空间。这是线性映射的一个重要的概念，什么是线性映射的核空间呢？简单的说，就是映射到零的原像的集合，记作KER。用正比例函数来类比，显然当k不等于0时，它的核是零空间，当k为零时，它的核空间是整个R。

> 有时候需要判定一个线性映射是不是单射，按照定义来还是没那么好证的，这时我们可以从它的核来判定，只要它的核是零，那么这个线性映射必然是单射。

> 2)线性映射的像。当自变量取遍整个定义域时，它的像的取值范围成为一个线性子空间，称为像空间，记作IM。

> 3)线性映射的矩阵表示。一个抽象的线性映射应如何'解析'的表达出来呢？这个表达式写出来就是一个矩阵，且这个矩阵依赖于基的选择。也就是说在不同的基下，线性映射有不同的矩阵。基有无穷个，相应的矩阵有无穷个。这就给用矩阵研究线性映射带来了麻烦。

> 幸好我们有相似矩阵。同一个线性映射在不同的基下的矩阵是相似关系，相似不变量有秩，行列式，迹，特征值，特征多项式等。所以可以通过相似矩阵来研究线性映射的秩，行列式，迹，特征值，特征多项式等性质。

> 线性映射的矩阵有无穷多，那么这其中有哪些是值得关注的呢？第一就是标准正交基下的矩阵了，这也是最常见的。

> 然而一个线性映射的矩阵在标准正交基下可能特别复杂，所以需要选择一组特殊的基，让它的矩阵在这个基下有最简单的矩阵表示。如果存在这样的基，使得线性映射的矩阵为对角矩阵，则称这个线性映射可对角化。

> 然而是不是所有线性映射都可以对角化呢，遗憾的是，并不是。那么就要问，如果一个线性映射不能对角化，那么它的最简矩阵是什么？这个问题的答案是若尔当标准型。可以证明，在复数域上，任何线性映射都存在唯一的若尔当标准型。

# 线性空间
1. 假定有一个n阶方阵的集合`$\boldsymbol{R}^{n*n} = \{方阵A,方阵B...\}$`，然后针对这个集合定义加法和数乘运算法则，
使得：
- 加法满足交换律、结合律，存在加法零元和负元； 
- 数乘中的数来自数域`$\boldsymbol{F}$`，数乘运算满足结合律、分配律，存在数乘的单位元；
那么就称  `$\boldsymbol{R}^{n*n}$`  这个集合为数域 `$\boldsymbol{F}$` 上的一个线性空间。

也就是说，线性空间本质上是一个满足了一定运算法则的集合。


## 线性组合/线性表出
假定有一个线性空间 `$\boldsymbol{V} = \{\boldsymbol{\alpha_1} , ..., \boldsymbol{\alpha_m}\}$`，它是一个向量的集合。
如果有一个向量`$\boldsymbol{\beta}=k_1\boldsymbol{\alpha_1}+...+k_m\boldsymbol{\alpha_m}$`，则称`$\boldsymbol{\beta}$`为`$\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}$`的一个线性组合，或者称`$\boldsymbol{\beta}$`可由`$\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}$`线性表出。

## 线性相关
设`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`为线性空间`$\boldsymbol{V}$`中的一个向量组（也就是说是集合`$\boldsymbol{V}$`的一个子集），如果存在一组不全为零的数`$k_1, ..., k_m$`，使得：`$\sum_{i=1}^{m}{k_i\boldsymbol{\alpha_i}}=\boldsymbol{0}$`，则称向量组`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`线性相关。
若`$\sum_{i=1}^{m}{k_i\boldsymbol{\alpha_i}}=\boldsymbol{0}$` => `$k_1=k_2=...=k_m=0$`，则称向量组`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`线性无关。

## 唯一线性表出定理
如果空间`$\boldsymbol{V}$`中的向量组`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`线性无关，且向量组`$\{\boldsymbol{\beta}, \boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`线性相关，那么`$\boldsymbol{\beta}$`可由`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`唯一线性表出。

## 基
假设集合`$\boldsymbol{V}$`是在数域`$\boldsymbol{F}$`上形成线性空间的，`$\boldsymbol{V}$`上有n个线性无关的向量`$\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}$`，如果说`$\boldsymbol{V}$`上所有的向量（也就是集合的元素）都可以由`$\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}$`线性表出，则称`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`为`$\boldsymbol{V}$`空间的一个基。

## 坐标
假定`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`为`$\boldsymbol{V}$`空间的一个基，若有一个`$\boldsymbol{V}$`中的向量`$\boldsymbol{\alpha}$`可以被这个基线性表出成: `$\boldsymbol{\alpha}=k_1\boldsymbol{\alpha_1}+k_2\boldsymbol{\alpha_2}+...+k_n\boldsymbol{\alpha_n}$`，那么这个系数组成的向量`$\boldsymbol{k}=[k_1, k_2, ..., k_n]^T$`就称为`$\boldsymbol{\alpha}$`在基`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`上的一个坐标。

## 维度
如果`$\boldsymbol{V}$`的基是一个含有n个元素的集合，那么就称`$\boldsymbol{V}$`为一个n维线性空间。记作`$dim\boldsymbol{V}=n$`。如果`$\boldsymbol{V}$`的基含有无穷多个元素，就称`$\boldsymbol{V}$`为无限维的。

## 基与坐标变换
通常情况下，一个向量在不同的基下的坐标是不同的。
设`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`与`$\{\boldsymbol{\beta_1},...,\boldsymbol{\beta_n}\}$`是`$\boldsymbol{V}$`中的两组基，则会有下面的运算关系：
$$
\boldsymbol{\beta_i}=p_{1i}\boldsymbol{\alpha_1}+p_{2i}\boldsymbol{\alpha_2}+...+p_{ni}\boldsymbol{\alpha_n}
=\left[\boldsymbol{\alpha_1}, \boldsymbol{\alpha_2}, ..., \boldsymbol{\alpha_n}\right] \left[\begin{array}{c}
p_{1i}\\
p_{2i}\\
...\\
p_{ni}
\end{array}\right]
$$
同理，扩展一下任意`$\boldsymbol{\beta_i}$`的情况，我们可以得到下面的矩阵等式：
$$
\left[
  \boldsymbol{\beta_1}, \boldsymbol{\beta_2}, ..., \boldsymbol{\beta_n}
  \right]

  = \left[ \boldsymbol{\alpha_1}, \boldsymbol{\alpha_2}, ..., \boldsymbol{\alpha_n} \right]

  \left[\begin{array}{cccc}
  p_{11}, p_{12}, ..., p_{1n} \\
  p_{21}, p_{22}, ..., p_{2n} \\
  ..., ..., ..., ..., \\
  p_{n1}, p_{n2}, ..., p_{nn}\\
    \end{array}\right]
$$
那么，右边👉这个大矩阵
$$
\boldsymbol{P} =  \left[\begin{array}{cccc}
  p_{11}, p_{12}, ..., p_{1n} \\
  p_{21}, p_{22}, ..., p_{2n} \\
  ..., ..., ..., ..., \\
  p_{n1}, p_{n2}, ..., p_{nn}\\
    \end{array}\right]
$$
就是从基`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`到基`$\{\boldsymbol{\beta_1},...,\boldsymbol{\beta_n}\}$`的过渡矩阵。
也就是说：
$$
\left[\boldsymbol{\beta_1}, \boldsymbol{\beta_2}, ..., \boldsymbol{\beta_n}\right] = \left[\boldsymbol{\alpha_1}, \boldsymbol{\alpha_2}, ..., \boldsymbol{\alpha_n}\right]\boldsymbol{P}
$$
过渡矩阵必定是可逆矩阵。这是为什么呢？简单来想，基`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`可以通过过渡矩阵线性变换到`$\{\boldsymbol{\beta_1},...,\boldsymbol{\beta_n}\}$`；那么也一定是可以变回来的。加法运算与数乘运算都是有逆元的。

## 欧式空间
首先，欧式空间是一个线性空间。它在实数域内满足向量加法的交换律、结合律、有零元和逆元；数乘运算满足交换律、结合律、有单位元。除了加法运算和数乘运算，欧式空间还定义了内积运算，它支持交换律、分配律，还有一些特殊的数乘运算规则（就是中学学的那些数乘运算规则）。那么这个线性空间就升级成了一个欧式空间。
（猜测）欧式空间这种代数结构应该是用来总结早期的几何学内容的。

## 子空间
设`$\boldsymbol{W}$`是空间`$\boldsymbol{V}$`的一个子集，如果这个子集满足下面两个条件：
(1)对于`$\boldsymbol{W}$`中的任意两个向量`$\boldsymbol{\alpha}, \boldsymbol{\beta}$`，两者的和也在这个`$\boldsymbol{W}$`中；
(2)对于`$\boldsymbol{W}$`的任意一个向量`$\boldsymbol{\alpha}$`，如果定义`$\boldsymbol{V}$`时配套的数域`$\boldsymbol{F}$`上的任意一个数k,都可以使得`$k\boldsymbol{\alpha}$`还属于这个`$\boldsymbol{W}$`子集；
那么就称`$\boldsymbol{W}$`为`$\boldsymbol{V}$`的一个子空间。
用人话说就是，子集`$\boldsymbol{W}$`对于加法和数乘自闭环。
显然，子空间的维数小于等于原空间。


### 子空间的张成
1. 设`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r}\}$`为`$\boldsymbol{V}$`中的一组向量，我们定义
$$
span\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r}\} \triangleq  \{k_1*\boldsymbol{\alpha_1},...,k_n*\boldsymbol{\alpha_r} | \forall k_i \in F\}
$$
称`$span\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r}\}$`为由`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r}\}$`张成的子空间。

理解起来比较简单直观，`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`中的任意向量在`$\boldsymbol{V}$`中对于加法和数乘操作都是封闭的。

### 子空间基的扩充
设`$\boldsymbol{W}$`为线性空间`$\boldsymbol{V^n}$`的子空间，`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m}\}$`为`$\boldsymbol{W}$`的一组基，则可将它扩充`$\boldsymbol{V^n}$`的一组基`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_m},\boldsymbol{\alpha_{m+1}},..., \boldsymbol{\alpha_n}\}$`。
这是在说子空间的基与空间的基之间的关系。

### 子空间的交、和运算
设`$\boldsymbol{W_1}$`与`$\boldsymbol{W_2}$`都是`$\boldsymbol{V}$`的子空间。
1. 那么交运算的定义为:

$$
\boldsymbol{W_1} \cap \boldsymbol{W_2} \triangleq \{\alpha \in \boldsymbol{V} | \alpha \in \boldsymbol{W_1} 且 \alpha \in \boldsymbol{W_2}\}
$$

2. 和运算定义为:

$$
\boldsymbol{W_1} + \boldsymbol{W_2} \triangleq \{\alpha \in \boldsymbol{V} | \alpha=\alpha_1+\alpha_2, \alpha_1 \in \boldsymbol{W_1} 且 \alpha_2 \in \boldsymbol{W_2}\}
$$

显然，`$\boldsymbol{W}$`的两个子空间的交、和结果依然是`$\boldsymbol{W}$`的子空间。

### 子空间的和与张成
如果说:`$\boldsymbol{W_1}$`是由`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r}\}$`张成的，而`$\boldsymbol{W}$`是由`$\{\boldsymbol{\beta_1},...,\boldsymbol{\beta_s}\}$`张成的，那么`$\boldsymbol{W}$`与`$\boldsymbol{W}$`的和是由`$\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_n}\}$`与`$\{\boldsymbol{\beta_1},...,\boldsymbol{\beta_s}\}$`一起张成的。
$$
\boldsymbol{W_1}= span\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r}\},\boldsymbol{W_2} = span\{\boldsymbol{\beta_1},...,\boldsymbol{\beta_s}\}，则:
\boldsymbol{W_1} + \boldsymbol{W_2} = span\{\boldsymbol{\alpha_1},...,\boldsymbol{\alpha_r},\boldsymbol{\beta_1},...,\boldsymbol{\beta_s}\}
$$

### 维数公式
设`$\boldsymbol{W_1}$`与`$\boldsymbol{W_2}$`都是`$\boldsymbol{V}$`的子空间，那么
$$
dim(\boldsymbol{W_1}+\boldsymbol{W_2}) + dim(\boldsymbol{W_1} \cap \boldsymbol{W_2}) = dim\boldsymbol{W_1} + dim\boldsymbol{W_2}
$$
这个形式与集合论中集合的势的规律是一样的，应该是这些数学家在特意的凑规律。
证明的思路依靠下面三条:
1. 维数的值等于基中的向量个数;
2. 基是一个空间的极大线性无关组;
3. 线性无关是说一组向量，他们的数乘之和为0，这些数乘操作所有取的数字都不是0;