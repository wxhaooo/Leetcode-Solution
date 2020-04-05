# A-6-Middle-ZigZag-Conversion

## 考点

* 字符串处理

## 题解

[参照这个题解](https://leetcode-cn.com/problems/zigzag-conversion/solution/z-zi-xing-bian-huan-by-leetcode/)

这个思路有些巧妙，百度2020年春招实习生C++研发岗有一道类似的题目

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows==1) return s;
        std::string ans="";
        int row = std::min(numRows,(int)s.size());
        std::vector<std::string> row_ans(row,std::string(""));

        bool reverse_flag=false;

        for(int i=0,cur_row=0;i<s.size();i++){
           row_ans[cur_row]+=s[i];
           if(cur_row==0||cur_row==numRows-1) reverse_flag=!reverse_flag;
           cur_row+=reverse_flag?1:-1;
           
        }
        for(auto& ss:row_ans)
            ans+=ss;

        return ans;
    }
};
```