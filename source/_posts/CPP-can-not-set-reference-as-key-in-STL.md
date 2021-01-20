---
title: C++ can not set reference as key in STL
date: 2021-01-09 15:58:32
tags: Tech
---

在写图的算法，尝试将Node/Edge的对象作为key存入unordered_map，发现一系列问题。
首先，如果要追求 
```
  std::unordered_map<Node, int> data;
```
这种写法(类似Java的那种风格)，得做两件事情:
1. 为Node类型重载 == 运算符，如果不愿意，可以选择未Node类型实现 () 运算符;
2. 实现一个自定义的供Node类型使用的hash方法，比如简单一点的:
  ```
  class MyHash {
   public:
    std::size_t operator() (const Node& node) const {
      return node.id;
    }
  }
  ```
3. 剩下的事情就是在申明unordered_map时注意一下，将hash函数传进去就行: 
```
std::unordered_map<Node, int, MyHash> data;
```


后来想尝试下，能不能向Java那样，使用地址值来做为hash值呢?写了个最简单的demo:
```
#include <iostream>
#include <string>
#include <unordered_map>

using namespace std;

class A {
private:
  std::string data_;
public:
  A() {
    data_ = "hello world";
  }
  bool operator== (const A& b) const {
    return (const A*)this == &b;
  }

};
class Myhash {
  public:
    std::size_t operator()(const A& b) const {
      return (std::size_t)&b; 
    }
};

int main() {

  A t;
  std::unordered_map<A&, int, Myhash> data;
  data[t] = 1;
  for (auto elem : data) {
    std::cout<<&(elem.first)<<", "<<elem.second<<std::endl;
  }
  std::cout<<data[t]<<std::endl;
  return 0;
}
```
到这里时，编译器报错了。google给出的答案是: STL中各种map(unordered_map, std::map)都是通过拷贝传值作为key，无法使用引用作为key。(ref: https://stackoverflow.com/questions/3235927/reference-as-key-in-stdmap)


怎么做到像Java那样自定义对象做key呢?几种可行的思路:
1. 舍弃Node做key，改用Node*;
2. 设计一个较好的hash函数，使得即使是拷贝传值的两个对象，也可通过hash值认为是同一份
3. 使用C++11引入的std::reference_wrapper
