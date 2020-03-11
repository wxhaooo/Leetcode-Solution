# 17-Middle: Letter Combinations of a Phone Number

## 考点

* 简单回溯，不需要剪枝

## 题解

```cpp
class Solution {
private:
    std::map<char,std::string> dict;
    std::string local_ans;
    std::vector<std::string> ans;
public:
    vector<string> letterCombinations(string digits) {
        if(digits.empty()) return std::vector<string>();
        
        dict={{'2',"abc"},{'3',"def"},{'4',"ghi"},{'5',"jkl"},{'6',"mno"},
        {'7',"pqrs"},{'8',"tuv"},{'9',"wxyz"}};

        int n = digits.size();

        Solve(0,digits);

        return ans;

    }

    void Solve(int cur,std::string& digits)
    {
        if(cur>=digits.size()){
            ans.push_back(local_ans);
            return;
        }

        char cur_ch=digits[cur];
        for(auto& ch:dict[cur_ch]){
            local_ans.push_back(ch);
            Solve(cur+1,digits);
            local_ans.pop_back();
        }
    }
};
```