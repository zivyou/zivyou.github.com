---
title: spring-boot-principle
date: 2025-12-02 10:46:10
tags: TECH
---


<!-- vim-markdown-toc GFM -->

* [Spring Boot Framework](#spring-boot-framework)
    * [语法](#语法)
        * [sealed/non-sealed](#sealednon-sealed)
    * [概念](#概念)
        * [SpringApplication](#springapplication)
        * [ApplicationContext](#applicationcontext)
    * [机制](#机制)
        * [SPI](#spi)
        * [自动装配](#自动装配)
        * [配置](#配置)

<!-- vim-markdown-toc -->

# Spring Boot Framework

## 语法
### sealed/non-sealed
Java 17中新引入的关键字,可以翻译为密封类/非密封类. 我们都知道, Java中有一个关键字final, 在修饰类时可以用来约束当前类不允许被其他类继承. 但是如果一个类允许被其他类继承,但是只能由某些子类继承,就需要使用sealed...permits语法了: 被sealed修饰的类, 只能被permits修饰的类继承, 同时,被permits修饰的类还必须在定义时声明自己是sealed、non-sealed还是final. 所以, 新引入的这个特性是在提供一种继承约束的能力.
这里面有一个问题: 被sealed修饰的类和被permits修饰的类需要互相引用,为了避免循环依赖问题,被sealed修饰的类和被permits修饰的类需要放在同一个包下.

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

### 自动装配
SpringBoot是如何做到让大部分的组件只需要一行配置引入后即可开箱即用呢?这就是SpringBoot自动装备机制提供的能力. 一般情况下, 我们可以引入一个spring-boot-starter-*包(一般官方的是这么命名) 或者*-spring-boot-starter包(一般非官方的是这么命名)来直接启用一个功能. 比如, 我们可以引入spring-boot-starter-actuator包,来启用GET /actuator/*等一系列系统运行时检查接口. 那么具体是怎么实现的呢?
1. spring-boot-starter-actuator包的内容:
非常简单,里面没有任何Java代码,只有一个gradle依赖描述: spring-boot-starter-actuator会引用spring-boot-health这个包; (当然也会引用其他的几个包,我们这里只关心这一个);
2. spring-boot-heatlh的内容:
观察spring-boot-health的resource/META-INFO/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports中的内容,可以知道spring-boot-health是通过SPI机制提供了一系列的AutoConfiguration类, 这些类就是自动装配的入口了. 这里spring-boot-health暴露了很多AutoConfiguration类,这里我们看一个: HealthEndpointAutoConfiguration, 这个类是给我们的服务提供GET /actuator/heatlh/*接口的配置类. 
HealthEndpointAutoConfiguration类的内容,就是自动装配设计的核心技术了,比如使用各种@ConditionOnXXX注解来声明类引入的约束, 这些约束/规则会最终确定SpringBoot内部会存在哪些Bean, 这也是「装配」一词的内涵.

### 配置
spring-boot中的配置变量可以有多种配置方式, 但是他们之间有优先级之分. 具体来说, 优先级从低到高依次为:
1. java命令行'--'参数, 即在最后执行java -jar 时以--key=value的方式设定的参数. 优先级最高, 可以override其他所有的参数;
2. 操作系统的环境变量: SPRING_DATASOURCE_URL会自动转换成spring.datasource.url变量;
3. java命令行 '-D'参数: 比如java -Dspring.profiles.active=prod -jar app.jar;
4. 外部的application-xxx.yaml: 一般额外设置application-xxx.yaml,都是用来在部署时通过spring.profiles.active手动激活使用的, 在激活后, application-xxx.yaml中变量会覆盖application.yaml中的变量;
5. 外部的application.yaml: 比如应用服务外的config文件夹指定的application.yaml;
6. 应用服务内部的application-xxx.yaml;
7. 应用服务内的application.yaml;
8. 应用服务内部通过@PropertySource注解执行的application.yaml;
8. spring-boot框架内部设置的默认值;
