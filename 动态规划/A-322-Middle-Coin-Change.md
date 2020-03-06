# 322-Middle: Coin Change

## 考点

* 动态规划
> 集合问题的动态规划，即动态规划的元素与某个集合相关
> __注意学习动态规划带最小值和集合时的处理情况以及代码怎么写__


## 题解

```cpp
//有着正确的dp的解法
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(coins.size() == 0) return -1;
        if(amount == 0) return 0;

        int max_limit = std::numeric_limits<int>::max();
        //用long long的原因是max+1可能超出Int,也可以初始值直接设为amount+1，因为最大值也不过amount+1
        std::vector<long long> dp(amount + 1,max_limit);
        dp[0] = 0;

        for(int i=1;i<dp.size();i++){
            for(auto& coin:coins){
                if(i-coin>=0) 
                    dp[i] = std::min(dp[i-coin] + 1,dp[i]);
            }
        }
        //最大值若设为amount+1，则为dp[amount]==amount+1
        return dp[amount]==max_limit ? -1:dp[amount];
    }
};
```

```cpp
//递归方法写的实验性质的记忆化
class Solution {
private:
    //记忆化的存储数据结构用map更为合适
    std::unordered_map<long long,long long> mem;
public:
    int coinChange(vector<int>& coins, int amount) {
        // base case
        if(amount < 0) return -1;
        if(amount ==0) return 0;

        //memory return
        if(mem.find(amount)!=mem.end()) return mem[amount];

        //general case
        long long local_ans = std::numeric_limits<long long>::max();
        for(long long i=0;i<coins.size();i++){
            long long tmp = coinChange(coins,amount-coins[i]);
            if(tmp==-1) continue;
            local_ans = std::min(local_ans,tmp + 1);
        }

        //memory
         return mem[amount]=local_ans;
    }
   
};
```

### 错解合集

```cpp
//dp的状态是对的，但是因为amount可能特别大，导致遍历的量特别大导致超时
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(amount==0) return 0;
        if(coins.size()==0) return -1;
       std::vector<int> dp(amount+1,amount+1);
       for(auto& coin:coins) {
           if(coin<=amount)
            dp[coin]=1;
       }
       for(int i=0;i<=amount;i++){
           for(int j=0;j<i;j++){
               dp[i] = std::min(dp[i],dp[j]+dp[i-j]);
           }
       }
       return (dp[amount]==amount+1) ? -1:dp[amount];
    }
};
```

```cpp
//这里dp的状态定义对了，但是dp的状态转移方程没有定义对导致出错，主要是对于动态规划中牵扯到集合不太了解
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(coins.size() == 0) return -1;
        if(amount == 0) return 0;

        // std::cout<<*std::max(std::begin(coins),std::end(coins))<<"\n";
        int max_coin = *std::max_element(std::begin(coins),std::end(coins));
        std::vector<int> table(max_coin + 1,-1);    
        for(int i=0;i<coins.size();i++)
            table[coins[i]]=1;

        std::vector<int> dp(amount + 1,std::numeric_limits<int>::max());
        dp[0] = 0;dp[1] = table[1];

        for(int i=2;i<dp.size();i++){
            if(i<=max_coin && table[i])
                dp[i] = 1;
            else{
                for(int j=2;j<i;j++){
                    if(dp[j]>0 && i-j<=max_coin && dp[i-j] > 0)
                        dp[i] = dp[i-j] + dp[j];
                }
                if(dp[i] == std::numeric_limits<int>::max())
                    dp[i] = -1;
            }
        } 
        return dp[amount];
    }
};
```

