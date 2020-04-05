# 16-Middle:3-Sum Closest

## 考点

* 双指针
> 看见涉及数组的遍历就要想到用双指针是不是能更快一点

* 多和问题
> [两和问题](https://leetcode-cn.com/problems/two-sum/)是这类问题的基础，然后考虑[三和问题](https://leetcode-cn.com/problems/3sum/)，然后是这道题。都是把暴力解法的求和问题用双指针简化得到的

## 题解

```cpp
//双指针优化的接近三和问题，算法复杂度为O(n^2)
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        long long ans_value = std::numeric_limits<int>::max();
        std::sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            int start = i + 1, end = nums.size() - 1;
            while (start < end) {
                int diff = target - nums[start] - nums[end] - nums[i];
                if (diff < 0) {
                    ans_value = std::abs(diff) < std::abs(target - ans_value) ? target - diff : ans_value;
                    end--;
                }
                else if (diff > 0) {
                    ans_value = std::abs(diff) < std::abs(target - ans_value) ? target - diff : ans_value;
                    start++; 
                }
                else return target;

            }
        }
        return ans_value;
    }
};
```