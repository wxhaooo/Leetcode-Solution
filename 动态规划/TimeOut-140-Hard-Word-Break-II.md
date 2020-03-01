# 140-Hard: Word Break II

## 考点

* 动态规划
* 记忆化搜索
> 暴力解法自己是写出来了，但是不会写记忆化搜索，这个需要练习一下

## 题解

```cpp
//写不出记忆化搜索的原因是不知道该记什么东西，要记住暴力解的时候画成数，记录的就是每个状态需要的结果，这个需要练习下
class Solution {
    private:
    std::map<int,std::vector<std::string>> table;
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        std::unordered_set<std::string> dict(wordDict.begin(),wordDict.end());
        return WordBreakRecursive(s,dict,0);
    }
private:
    std::vector<std::string> WordBreakRecursive(std::string& s,std::unordered_set<std::string>& dict,int start)
    {
        if(table.find(start)!=table.end()){
            return table[start];
        }
        std::vector<std::string> local_ans;

        if (start == s.size()) {
            local_ans.push_back("");
        }
       
        for(int i=start;i<=s.size();i++){
            if(dict.find(s.substr(start,i-start+1))!=dict.end()){
                auto tmp = WordBreakRecursive(s,dict,i+1);
                for(auto& tmp_ele:tmp)
                {
                    tmp_ele=s.substr(start,i-start+1) +((tmp_ele=="") ? "":" ") + tmp_ele;
                }
                local_ans.insert(local_ans.end(),tmp.begin(),tmp.end());
            }
        }
        return table[start]=local_ans;
    }
};
```

```cpp
//暴力解法
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        std::vector<std::string> ans;
        std::string prev_str="";
        std::unordered_set<std::string> dict(wordDict.begin(),wordDict.end());
        WordBreakRecursive(s,dict,0,s.size()-1,ans,prev_str);
        return ans;
    }
private:
    bool WordBreakRecursive(std::string& s,std::unordered_set<std::string>& dict,
                            int start,int end,std::vector<std::string>& ans,
                            std::string& prev_str)
    {
        if(start > end){
            ans.push_back(prev_str.substr(1,prev_str.size()-1));
            return true;
        }

        for(int i=start;i<=end;i++){
            if(dict.find(s.substr(start,i-start+1))!=dict.end()){
                std::string tmp_str= prev_str + " "+ s.substr(start,i-start+1);
                WordBreakRecursive(s,dict,i+1,end,ans,tmp_str);
                // if(!WordBreakRecursive(s,dict,i+1,end,ans,tmp_str))
                    // return false;
            }
        }
        return false;
    }
};
```