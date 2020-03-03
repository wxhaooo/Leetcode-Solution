# 169-Simple: Majority Element

## 考点

* 哈希表
> 使用hash table，时间和空间复杂度为$O(n)$
* 分治
> 这个代码没写，分治的复杂度为$O(n\;log\;n)$
* 摩尔投票法
> 如果必然存在某个元素是主元素，那么可以把所有元素分为两类，一类是主元素，一类不是主元素。所以遍历整个数组，如果是主元素，则计数++，否则--。主元素初始化为s[0]，当当前主元素计数小于等于0时，当前主元素替换为下一个元素并继续这个过程。__可以看做好几个不同军队抢夺一个高地，他们一对一消耗
因为有个军队超过了n/2，经过消耗后，他还有人活着。__

## 题解

```cpp
//使用hash table的做法
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        std::unordered_map<int,int> table;
        int n = nums.size();
        for(auto& num:nums){
            table[num]++;
            if(table[num]>n/2)
                return num;
        }
        return -1;
    }
};
```