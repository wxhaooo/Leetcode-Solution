# 279-Middle: Perfect Squares

## 考点

* 动态规划
* 完全平方数的快速判断（索引访问的学习）

### 学到的东西

* 动态规划几乎一定可以优化，能优化的时候一定要优化
* 完全平方数的快速判断
>```cpp
>bool IsPerfectSquare(int& n){
> //不能用sqrt(n)这种直接判断，因为我们需要 i * i == n都是整数类型，sqrt()一般实现上只有浮点型，会有误差
> //为了能访问到sqrt(n)，使用sqrt(n+1)作为界标比较合适
>        for(int i=0;i<sqrt(n+1);i++){
>            if(i * i == n)
>            return true;
>        }
>        return false;
>    }
>```

### 出现的问题

* DP的状态战役方程都写出来了，但是效率不够高

## 题解

```cpp
//注意到要求的数一定由完全平方数组成，且组成这个数的完全平方数一定小于sqrt(i)，则类似于完全平方数的判定，只需要检测<=sqrt(i)的范围即可。又注意到在这个范围内我们只要取一个完全平方数j*j，然后让剩余的量dp[i-j*j]最小即可,所以dp变为
//dp = 1 ,如果i是完全平方数
//dp = min{dp[i-j*j]+dp[j*j],dp[i]} j~[1,sqrt(n)]{ dp[j*j]一定是完全平方数，所以dp[j*j]=1恒成立}
//故复杂度变为O(n*sqrt(n))
class Solution {
public:
    int numSquares(int n) {
        if(n==0) return 0;
        int max_limit = std::numeric_limits<int>::max();
        std::vector<int> dp(n+1,max_limit);
        dp[0]=0;
        for(int i=1;i<=n;i++){
            if(IsPerfectSquare(i)){
                dp[i] = 1;
                continue;
            }
        
            for(int j =1;j<sqrt(i+1);j++){
                dp[i] = std::min(dp[i-j * j]+dp[j * j],dp[i]);
            }
        }

        return dp[n];
    }
private:
    bool IsPerfectSquare(int& n){
        for(int i=0;i<sqrt(n+1);i++){
            if(i * i == n)
            return true;
        }
        return false;
    }
};
```

```cpp
//Timeout的第一版,时间复杂度为O(n^2)
//dp[i]定义为计算i需要的最小完全平方数的个数
//dp[i] = 1 ，如果i是完全平方数
//dp[i] = min{dp[i-j]+dp[j],dp[i]} j~[1,n) 如果i不是完全平方数则扫描前面进行两两组合取最小的
class Solution {
public:
    int numSquares(int n) {
        if(n==0) return 0;
        int max_limit = std::numeric_limits<int>::max();
        std::vector<int> dp(n+1,max_limit);
        dp[0]=0;
        for(int i=1;i<=n;i++){
            if(IsPerfectSquare(i)){
                dp[i] = 1;
                continue;
            }
        
            for(int j =1;j<i;j++){
                dp[i] = std::min(dp[i-j]+dp[j],dp[i]);
            }
        }

        return dp[n];
    }
private:
    bool IsPerfectSquare(int& n){
        for(int i=0;i<sqrt(n+1);i++){
            if(i * i == n)
            return true;
        }
        return false;
    }
};
```