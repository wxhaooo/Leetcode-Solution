# 139-Middle: Word Break

## 考点

* 非整数类型的（布尔类型）的动态规划
  
### 学到的东西

* std::set()/std::map()/std::unordered_X()都接受一组迭代器作为初始化的输入
* 动态规划的bool类型
* __暴力回溯法一定要掌握，大不了用记忆化搜索，一些题都能A掉__
暴力回溯法要会画图，一次回溯就是一个状态，状态画对了，记忆化搜索就好改了
## 题解

```cpp
//动态规划解法
//dp[i]定义为长度为i的序列能否被拆分为字典中的条目
//则状态转移方程为dp[i] = dp[j] && (s[j,i-j] in dict) j ~[0,i)
//这个状态转移方程的意思是存在长度为j的子串dp[j]以及剩余子串
//s[j,i-j]在字典内则拆分成功，dp[i]=true,否则dp[i]=false
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        std::set<std::string> words;
        for(auto& word:wordDict)
            words.insert(word);
        
        std::vector<bool> dp(s.size() + 1,false);
        dp[0] = true;
        for(int i=1;i<dp.size();i++){
            for(int j=0;j<i;j++){
                if(dp[j] && words.find(s.substr(j,i-j))!=words.end()){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp.back();
    }
};
```