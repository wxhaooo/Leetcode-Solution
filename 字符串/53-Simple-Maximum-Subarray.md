# 53-Simple: Maximum Subarray

## 考点

* 最大子序列问题的变种（有负数情况）
难点也在于考虑负数情况下的最大子序列问题

## 题解

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int ans=0;
        int cur_max = std::numeric_limits<int>::min();
        //先试着找第一个正数（找到就是最大子序列问题）
        //找不到就记录最大的那个负数，即为题目的解
        int start=0;
        for(;start<nums.size();start++){
            if(nums[start] >= 0){
             cur_max = nums[start];
            break;****
            }
            
            if(nums[start]>cur_max)
            cur_max = nums[start];
        }
        if(start==nums.size() && cur_max < 0)
            return cur_max;
        //最大子序列问题的解法
        for(;start<nums.size();start++){
            if(ans + nums[start] >= 0){
                ans = ans + nums[start];
                cur_max = cur_max < ans ? ans : cur_max;
            }
            else{
                ans = 0;
            }
        }
        return cur_max;
    }
};
```