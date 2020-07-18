---
title: leetcode-cn-172
date: 2020-06-29 00:46:37
tags: leetcode
---
- 寻找一个数的阶乘中，末尾位数中连续的0的个数
- 目前的思路：将任意一个合数进行质因数分解，分解集中的5的个数，就是此数能够贡献的0的数量；
    而对于一个质数，可以直接略过，因为它不可能贡献出0（当然除了5本身）；
```math
    C=2^{k_1}*3^{k_2}*5^{k_3}*7^{k_4}...
```
- 对于阶乘，
```math
n! = 1*2*3*4*5*...*n
```
每一个乘数都需要做质因数分解并计算分解集中5出现的次数。
```
class Solution {
public:
    int rawTrailingZeroes(int a, int b){
        if (b - a < 5){
            int count = 0;
            for (int i=a; i<=b; i++){
                int j = i;
                while (!(j%5)){
                    count++;
                    j = j/5;
                }
            }
            return count;
        }

        return rawTrailingZeroes(a, (a+b)/2)+rawTrailingZeroes((a+b)/2+1, b);
    }
    int trailingZeroes(int n) {
        return rawTrailingZeroes(1, n);
    }
};
    
```

- 提交，发现运行超时

-------------------------------------------------------------


- 设`$f(n)$`为n的阶乘中末尾0的数量，则有：
    `$f(n) = n/5 + f(n/5)$`

- 数学归纳法证明：
    1. 对于`$n<5$`, 显然有`$f(n) = n/5 + f(n/5) = 0$`成立；
    2. 假设对于`$n>5$`，有`$f(n) = n/5 + f(n/5)$`成立，则对于另一个数`$m$`,有：
        - 若`$m(m\in[n+1, n*5-1])$`，根据质因数分解公式，`$n \to m$`中的任意数，都不会有新的质因数5出现。
        - 若`$m=n*5$`,根据质因数分解公式，相对于`$n$`,`$m$`出现了新的质因数5，因此有`$f(m) = 1+f(n) = m/5+f(m/5)$`。
    3. 综上，`$f(n) = n/5 + f(n/5)$` 成立。

