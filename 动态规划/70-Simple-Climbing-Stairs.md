# 70-Simple: Climbing Stairs

## 考点

* 动态规划
* 记忆化递归
* 斐波那契数列

## 题解

```cpp
//动态规划方法，同91题解法
class Solution {
public:
    int climbStairs(int n) {
        if(n==0) return 0;
        if(n==1) return 1;
        int prev1 =1,prev2 =1;
        for(int i=2;i<=n;i++){
            int cur = prev1 + prev2;
            prev2 = prev1;
            prev1 = cur;
        }
        return prev1;
    }
};
```