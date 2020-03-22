# 215-Middle: Kth Largest Element in an Array

## 考点

* 排序与 __快速选择算法__
> 这个快速选择算法是第一次见，看起来还是挺有用的，学习一下。
> 快速选择算法的套路和快排差不多，不过快排的平均时间复杂度是$O(n \cdot \log{n})$，快速选择的平均时间复杂度是$O(n)$，两个的最坏时间复杂度都是$O(n^2)$

## 题解

```cpp
//快速选择算法

```


### 排序方法

```cpp
//排序方法，比较naive
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        std::sort(nums.begin(),nums.end(),std::greater<int>());
        for(int i=0;i<nums.size();i++)
            if(i==k-1) return nums[i];
        return -1;
    }
};
```