---
title: or-tools
date: 2025-02-09 10:37:07
tags: TECH
---

# INSTALL
ubuntu 22.04，直接导入cmake会缺少一些依赖：
1. 缺少absl库
`sudo apt install libabsl-dev` ~事实证明这样安装不行，这样安装会缺少cmake配置文件，后面有的库安装会找absl的cmake~所以只能通过源码自己编译、安装，唉，简直了；
这里还是用checkinstall安装；


2. 缺少protobuf
`sudo apt install libprotobuf-dev`
3. 缺少HIGHS库
查了一下，这个是一个运筹学领域的线性优化求解器，官网： https://highs.dev/。简单看了一下官方推荐还是使用源码自编译安装；

## 安装highs库
下载源码后，cmake , make, make install这一套； 鉴于怕污染系统导致后面我的电脑软件管理混乱，这里直接上checkinstall来安装deb包了；

4. 缺少SCIP库
这个是一个Constraint Integer Programs问题求解器，官网https://www.scipopt.org/，提供了deb包供下载（太好了）

5. 缺少re2::re2库
这个库是google出品，主页：https://github.com/google/re2，看起来是一个C++的正则表达式库；
可恶的是没有提供deb包，只能通过源码去安装；
这里还是直接上checkinstall;
## 安装re2::re2
这个re2库依赖于absl::absl_check和absl::absl_log，这么搞下去估计要把google全家桶都安装上了；

6. 缺少Coin::CbcSolver库
主页https://github.com/coin-or/Cbc?tab=readme-ov-file，看起来也是一个整数域线性规划求解器。
试了一下通过apt-get安装的方式： sudo apt-get install  coinor-cbc coinor-libcbc-dev，居然可以用（官方的安装包中有cmake配置项，所以or-tools的cmake可以找到依赖）。

好了，至此可以编译or-tools了。
