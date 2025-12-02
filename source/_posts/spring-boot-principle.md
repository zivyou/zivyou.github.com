---
title: spring-boot-principle
date: 2025-12-02 10:46:10
tags: TECH
---

# Spring Boot Framework
## 概念
### SpringApplication
SpringApplication完整包名为:org.springframework.boot.SpringApplication. 是SpringBoot源码定义的一个类. 主要功能是管理服务启动过程的整个生命周期. 核心方法为一个void run(String... args)方法, 里面会做一些环境变量加载和解析、ApplicationContext初始化、Bean构造、启动web server等等动作, 本质上是将之前古早时期需要手动做的动作给自动化了, 让整个Spring应用运行更轻松.
### ApplicationContext
这是Spring框架定义的一个概念(不是SpringBoot框架), 也是我们常说的「容器」的概念的一种具体形式. 「容器』一词的标准定义应该是由interface org.springframework.beans.factory.BeanFactory定义的, 该interface定义了所有的Bean get/set方法.而ApplicatoinContext继承了BeanFactory接口: 是的, ApplicationContext自己也是一个interface, 除了BeanFactory定义的方法外, ApplicationContext还扩展了环境变量管理、MessageSource、事件机制等其他基础框架的功能. ApplicationContext的一个具体实现类为: org.springframework.context.support.GenericApplicationContext, 这个类聚合了DefaultListableBeanFactory, DefaultListableBeanFactory继承了AbstractAutowireCapableBeanFactory; AbstractAutowireCapableBeanFactory继承了AbstractBeanFactory; AbstractBeanFactoryFactory继承了BeanRegistrySupport; FactoryBeanRegistrySupport继承了DefaultSingletonBeanRegistry, DefaultSingletonBeanRegistry就是我们存储各个Bean单例对象的地方, 从源码可以看到,其实就是使用ConcurrentHashMap存储<name, Object>键值对. 



## 机制
### SPI
SPI(service provider interface), 是Java语言内置的一种「服务发现机制」.核心设计思想是服务调用方只提供接口定义和服务发现能力,来达成功能的模块化、插件化, 最终做到模块解耦的效果.
服务发现机制:
1. 接口实现方(也就是服务提供方)在包的META-INF/services/目录下创建配置文件(配置文件名是由接口定义方,也就是服务调用者定义的),里面写明提供某个接口的实现类是哪一个;
2. 调用调用方通过jdk提供的工具类:java.util.ServiceLoader来扫描已经加载的所有包中有哪些实现类;

SpringBoot的SPI机制: Springboot使用SPI机制的方法与Java标准的SPI方式不太一样, 主要是SpringBoot框架加载接口的具体实现时,并不是从META-INF/services目录下加载, 通常META-INF/services目录下定义的配置文件名会是接口的全类名. SpringBoot选择将接口定义在META-INF/spring.factories文件中,这样做的原因主要有两个:(1) springboot自身定义的接口太多了,放在META-INF/services中有点乱;而spring.factories文件中使用key-value的形式定义了所有的实现类; (2) springboot单独实现了SPI的服务发现能力,因此定义在spirng.factories中的实现,不仅支持对interface的实现,也支持对抽象类的继承,甚至注解的继承.
