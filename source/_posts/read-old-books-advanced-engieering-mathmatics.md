---
title: 旧书新读-《高等工程数学》
date: 2022-01-20 10:02:08
mathjax:
  enable: true
tags: books
---

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