---
title: leetcode-cn-131
date: 2020-06-29 00:48:24
tags: leetcode
---

- 字符串s, 假定前面的[0, a)都已经搜索过了，那么下面的[a, s.size())部分可以用这个策略：
```
i从[a, s.size())：
- 假如s.substr(a, i-a+1)不是回文串，i++继续寻找下一个；
- 假如s.substr(a, i-a+1)是回文串，可以将s.substr(a, i-a+1)推入我们的临时搜索结果集中，然后递归搜索剩下的；但是有个问题，i的后面就没有其他的字串s.substr(a, k-a+1)是回文串了吗?
  所以，在将s.substr(a, i-a+1)推入搜索结果集中，并调用递归进行剩下的搜索之后，还需要把s.substr(a, i-a+1)从临时结果集中弹出，让i++继续，寻找当前临时结果集下其他的可能回文串。
  故：
  
  void search(vector<vector<string>> &re, vector<string> tmpRe, string &s, int a, int b){
      if (a >= b){
          re.push_back(tmpRe);
          return;
      }
      for (int i=a; i<=b; i++){
          if (isPalindrome(s.substr(a, i-a+1))){
              tmpRe.push_back(s.substr(a, i-a+1));
              search(re, tmpRe, s, i+1, b);
              tmpRe.pop_back();   // 这一步就是所谓的回溯
          }
      }
  }
```
