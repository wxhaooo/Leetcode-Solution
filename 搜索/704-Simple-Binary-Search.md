# 704-Simple:Binary Search

## 考点

* 二分搜索
> 这道题非常简单，但是需要注意的是二分搜索的不同形式。普通的二分搜索只能搜到给定的值，但是改造的二分搜索能搜到相同值中特定位置的值，详情见[多种二分搜索的说明](https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-xiang-jie-by-labuladong/)


## 题解
### 多种二分搜索

```cpp
//最直接的二分搜索
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

//搜索到左边界的二分搜索
int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，收缩左侧边界
            right = mid - 1;
        }
    }
    // 最后要检查 left 越界的情况
    if (left >= nums.length || nums[left] != target)
        return -1;
    return left;
}

//搜索到右边界的二分搜索
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，收缩右侧边界
            left = mid + 1;
        }
    }
    // 最后要检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}
```
```cpp
//自己的题解
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int start=0,end=nums.size()-1;
        while(start<=end){
        int mid = start+(end-start)/2;
            if(target<nums[mid])
                end=mid-1;
            else if(target>nums[mid])
                start=mid+1;
            else
                return mid;
        }
        return -1;
    }
};
```