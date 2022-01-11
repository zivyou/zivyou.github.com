---
title: Spring中奇奇怪怪的注解 
date: 2021-12-29 14:22:55
tags:
---

# @Import
Spring用来做配置的几个重要注解之一。@Import可以将一个被注解的类生成一个Bean并交给容器管理，从这一点上看，@Import有着和@Bean相似的功能，但是@Import的能力更大。它可以提供自定义的导入方式、导入条件，甚至可以动态修改导入的Bean的属性、类型等等。
这个注解最早设计的时候，目标是为了替代远古时期的XML配置的Bean注入功能。类似于XML中的<import />。后来演化出了更多功能。
现在Import主要有三种用法:
1. @Import({})直接声明要注入到容器中的Bean的类;
2. @Import({})声明一个「实现了ImportSelector接口的类(假设为X)」; 在这个X类重写的selectImports接口中，声明要引入哪一些类; 这样绕一圈的目的是让开发者有办法自己过滤或者自己定义要引入哪一些类;
3. @Import({})声明一个「实现了ImportBeanDefinitionRegister接口的类(假设为Y)」;在这个Y类重写的

