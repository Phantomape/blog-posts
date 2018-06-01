---
title: Two Sum
tags: 
- Algorithm
categories: Algorithm
---
What's interesting is that if I try to put the number into the unordered_map before I check for numbers that satisfy my target, some test cases won't pass.

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> buf;
        vector<int> res;
        for (int i = 0; i < nums.size(); i ++) {
            if (buf.count(target - nums[i]) > 0 && i != buf[target - nums[i]]) {
                res.push_back(i);
                res.push_back(buf[target - nums[i]]);
                break;
            }
            buf[nums[i]] = i;
        }
        sort(res.begin(), res.end());
        return res;
    }
};
```