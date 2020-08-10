---
title: leetcode-cn-377
date: 2020-07-02 00:00:38
tags: leetcode
---
```
class Solution {
private:
  map<int, int> m;
public:
    int combinationSum4(vector<int>& nums, int target) {
      unsigned int re[target+1];
      for (int i=0; i<=target; i++) re[i] = 0;
      int cur = 0;
      while (cur ++ < target) {
        bool found = false;
        for (auto elem: nums) {
          if (elem <= cur) {
            re[cur] += re[cur-elem];
            if (elem == cur) found = true;
          }
        }
        if (found) re[cur]++;
      }
      return re[target];
    }
};

```

坑很深的一道题：
1. 一开始的时候没有细想，直接上搜索：
  search(nums, begin, target-nums[begin], count);
  search(nums, begin+1, target-nums[begin], count);
  search(nums, begin+1, target, count);
发现超时了，调试了一番，发现这种方法即使在target很小的时候，时间复杂度也十分惊人，可见是不可取的方法；
2. 想到对1中方法进行优化：利用map记录已经搜索出结果的中间过程。试验一番发现仍然不行，时间复杂度还是太高
3. 在网上搜索了别人的思路，发现正确做法是用dp，并且是自底向上的dp
4. 确实是一道好题：你以为我在第一层，其实我在第四层