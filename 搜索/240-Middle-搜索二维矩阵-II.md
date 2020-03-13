# 240-Middle: 搜索二维矩阵 II

## 考点

* 二分搜索
> 二分搜索要写熟练！！！！
* 分治和归并
> 这个分治比较难想到，学习一下，对于二维有序数组的查找可以这样干
* 巧妙方法
> [这种方法参考这个题解的解法二](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-5-4/)
## 题解

```cpp
//对每一行用二分法进行搜索
class Solution {
public:

    int BinarySearch(int start,int end,int k,vector<int>& nums)
    {
        while(start<=end)
        {
            int mid = start+(end-start)/2;  
            if(k<nums[mid]) end=mid-1;
            else if(k>nums[mid]) start=mid+1;
            else return mid;
        }
        return -1;
    }

    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0||matrix[0].size()==0) return false;
        int m = matrix.size(),n=matrix[0].size();

        for(int i=0;i<m;i++){
            if(target<matrix[i][0]) continue;
            int pos = BinarySearch(0,n-1,target,matrix[i]);
            if(pos!=-1 && target==matrix[i][pos]) return true;
        }
        return false;
    }
};


```