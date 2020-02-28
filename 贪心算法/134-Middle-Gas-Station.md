# 134-Middle: Gas Station

## 考点

* 贪心算法
* 动态规划【可以用动态规划来做】

## 题解

这个自己写了题解，参考[自己的题解](https://leetcode-cn.com/problems/gas-station/solution/dong-tai-gui-hua-fa-jie-jue-gai-wen-ti-by-yan-nan-/)
```cpp
//动态规划版
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int diff_gas = 0;
        for(int i=0;i<gas.size();i++)
            diff_gas +=(gas[i]-cost[i]);
        
        if(diff_gas < 0) return -1;
        
        int max_start_index = 0,max_sum= 0;
        int prev = 0,start_index = -1;
        for(int i=0;i<gas.size();i++){
            int cur = std::max(0,prev+gas[i]-cost[i]);
            if(cur > max_sum) {max_sum = cur;max_start_index = start_index;}
            if(prev==0 && cur >=0) start_index = i;
            prev = cur;
        }
        return start_index;
    }
};
```