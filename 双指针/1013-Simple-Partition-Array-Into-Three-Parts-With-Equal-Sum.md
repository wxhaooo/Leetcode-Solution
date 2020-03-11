# 1013-Simple: Partition Array Into Three Parts With Equal Sum

## 考点

* 数组操作
* 双指针法
> 使用双指针法需要注意到 __当找到一对满足条件的数组时，另一个数组则自动满足__，这点观察很重要。双指针法需要练习一下。


## 题解

[这种方法的题解见这里](https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/solution/c-bu-shi-yong-shuang-zhi-zhen-de-ling-yi-chong-jie/)
```cpp
//不使用双指针法的解法
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& A) {
        int n=A.size();
        int sum=std::accumulate(A.begin(),A.end(),0);
        if(sum%3!=0) return false;
        int target_sum = sum/3;
        int divide_num =0;
        for(int i=0,cur_sum=0;i<n;i++){
            cur_sum+=A[i];
            if(cur_sum==target_sum){divide_num++;cur_sum=0;}
        }

        return divide_num>=3?true:false;
    }
};
```

