---
title: leetcode-cn-39
date: 2020-06-29 00:49:44
tags: leetcode
---
- 非常标准的搜索+回溯
```

#include <vector>

using namespace std;

class Solution {
public:
    vector<int> data;
    vector<vector<int>> re;
    void dfs(int target, int start, vector<int>& output){
        if (target < 0) return;
        if (target == 0) {
            vector<int> tmp = output;
            sort(tmp.begin(), tmp.end());
            re.push_back(tmp);
            return;
        }

        for (int i=start; i<data.size(); i++){
            output.push_back(data[i]);
            dfs(target-data[i], i, output);
            output.pop_back();
        }
    }
    static int cmp(int &a, int &b){
        if (a > b)
            return 1;
        else
            return 0;
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        data = candidates;
        sort(data.begin(), data.end(), Solution::cmp); //here!!
        vector<int> output; 
        dfs(target, 0, output);
        return re;
    }
};
```

- 有意思的是，如果在开始dfs之前，先排序一把，能极大提高效率:-)