---
title: leetcode-cn-46
date: 2020-06-29 00:51:03
tags: leetcode
---

- 非常典型的一道搜索题
- 对于数组nums, 每次递归划分一个搜索范围，每次找这个范围的最后一个元素elem，将这个元素elem插入到已有搜索结果tmpRe的不通位置，再进行下一轮的搜索递归

```
class Solution {
public:
    void dfs(vector<vector<int> > &re, vector<int> & nums, int end, vector<int> tmpRe){
        if (end<0){
            re.push_back(tmpRe);
            return;
        }
        int elem = nums[end];
        for (int i=0; i<=tmpRe.size(); i++){
            vector<int> newTmpRe = tmpRe;
            newTmpRe.insert(newTmpRe.begin()+i, elem);
            dfs(re, nums, end-1, newTmpRe);
        }
    }
    
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int> > re;
        vector<int> tmpRe;
        dfs(re, nums, nums.size()-1, tmpRe);
        return re;
    }
};
```

- 另：此题的标准解法应当是使用回溯法解。
- 在上面的解法中，使用了一个很tricky的思想，及每次试探一个nums中的元素，我们都是从后面一个向前面一个试探。并且我们是向tmpRe的不同位置去插入这个元素。回溯的想法是，我每次都从nums中挑选一个，并且固定的加在tmpRe的最末尾。但是这样，我们每次都是去便利nums中的每一个元素，这就意味着有可能有元素被重复的挑选。比如nums = [1,2,3]，那这样的算出的结果中可能会出现[1,1,1]这样的数组。这时，我们可以额外使用一个used数组去记录，某一个位置i上的数是否已经被挑选到tmpRe中了。
- 回溯的大致解法如下：
```

class Solution {
public:
    void dfs(vector<vector<int>> &re, vector<int> &nums, vector<int> &tmpRe, int* used){
        if (tmpRe.size() == nums.size()){
            re.push_back(tmpRe);
            return;
        }


        for (int i=0; i<nums.size(); i++){
            if (used[i])
                continue;
            tmpRe.push_back(nums[i]);   //每次都添加到tmpRe的末尾
            used[i] = 1;
            dfs(re, nums, tmpRe, used);
            tmpRe.pop_back();    //回溯，对此次的选择反悔。这样此刻的这个nums[i]会在新的tmpRe末尾，相当于换了个位置
            used[i] = 0;
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> re;
        vector<int> tmpRe;
        int used[nums.size()+1];
        for (int i=0; i<nums.size()+1; i++)
            used[i] = 0;
        dfs(re, nums, tmpRe, used);
        return re;
    }
};


```
