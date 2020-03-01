# 268-Simple: Missing Number

## 考点

* __异或运算__
> 这个方法利用同一个数异或两次还是本身，先把每个数自己异或一次，然后在和数组中的元素异或一次，不是本身的就是缺失的数字。
* 数学方法

## 题解

```cpp
//数学方法，求0~n的累和，求数组的累和，相减就是缺失的数字
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        int total = std::accumulate(nums.begin(),nums.end(),0);
        return ((n+1)*n)/2 - total;
    }
};
```