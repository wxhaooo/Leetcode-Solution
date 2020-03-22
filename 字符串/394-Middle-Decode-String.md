# 394-Middle: Decode String

## 考点

* 字符串的处理


## 题解

这道题最需要注意的就是第二个实例，第一次做的时候没发现它是嵌套的导致出错。实际只要和处理表达式解析一样，当遇到 "]"的时候就开始弹出直到遇到 "["。又因为表达式一定合法。当处理完"[]"时再读取前面的数字字符串决定重复几次。然后把重复N次构成的新字符串再压入栈继续安装上面的方法处理即可。最后只要把栈从头到尾反向输出一次即可。为了方便起见，这里用的deque。

```cpp
//上面题解思路的实现
class Solution {
public:
    string decodeString(string s) {
        std::string ans="";
        std::deque<char> st;
        int num;
        for(int i=0;i<s.size();i++){
            if(std::isalnum(s[i])||s[i]=='['){
               st.push_back(s[i]);
            }
            else if(s[i]==']'){
                std::string local_ans="";
                while(!st.empty()&&st.back()!='['){
                    local_ans+=st.back();
                    st.pop_back();
                }
                st.pop_back();
                num=0; int base = 0;
                while(!st.empty()&&std::isdigit(st.back())){
                    num+=(st.back()-'0') * std::pow(10,base++);
                    st.pop_back();
                }
                std::string tmp ="";
                std::reverse(local_ans.begin(),local_ans.end());
                while(num--){tmp+=local_ans;}
                for(auto& ch:tmp){st.push_back(ch);}
            }
        }
        while(!st.empty()){ans+=st.front();st.pop_front();}
        return ans;
    }

};
```