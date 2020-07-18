---
title: leetcode-cn-376
date: 2020-06-28 23:25:29
tags: leetcode
---
```
class Solution {
public:
  int wiggleMaxLength(vector<int>& nums) {
    if (nums.size() <= 0) return 0;
    int len = nums.size();
    if (len == 1) return 1;
    int up = 1; int down = 1;
    for (int i=1; i<nums.size(); i++) {
      if (nums[i] > nums[i-1]) {
        up = down + 1;
      }

      if (nums[i] < nums[i-1]) {
        down = up + 1;
      }
    }

    return max(up, down);
  }
};
```
求摆动子序列的最大长度
有点类似于脑筋急转弯：假如a[i]-a[i-1]的结果用/和\来表示大于0和小于0，那么，就相当于求一个.../\/\/\/序列和.../\/\/\序列的最大长度
用变量up表示.../\/\/\/序列在遍历过程中的长度，down表示.../\/\/\序列在遍历过程中的长度
由于已经到i了，在i处时/还是\是明确的（nums[i]-nums[i-1]就知道了）：
1. 若在i处是/: 则在i处是以/结尾，即匹配.../\/\/\/序列, 其长度up=down+1; 
2. 若在i处是\: 则在i处是以\结尾，即匹配.../\/\/\序列，其长度down=up+1;