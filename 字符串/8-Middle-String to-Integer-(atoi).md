# 8-Simple: String to Integer (atoi)

## 考点

* 字符串对比以及字符类型判断

## 题解

```cpp
class Solution {
    //思路：start一定指向第一个有效数字，end一定指向最后一个有效数字
    //检查start，end是否是有效数字，不是则返回0
    //符号一定出现在有效字段的开始，内部的符号无效
public:
    int myAtoi(string str) {
        //长度为0的特殊情况
        if(str.size() == 0) return 0;
        //第一个字符是非有效字符
        if(!std::isspace(str[0]) && !std::isdigit(str[0]) 
        && str[0]!='+' && str[0]!='-') return 0;
        //start 是一个digit的位置
        int start = 0;
        while(start<str.size() && std::isspace(str[start])){start++;}
        int sign_index = -1;
        if(str[start] == '+' || str[start] == '-') 
        {
            sign_index = start;
            start++;
        }
        //保证start一定是第一个digit，否则返回0
        if(!std::isdigit(str[start])) return 0;

        int end = start + 1;
        int sign_num =0;
        while(end < str.size() && std::isdigit(str[end])){end++;}
        end--;

        //保证end是最后一个digit，否则返回0
        if(end == sign_index || end == str.size()) return 0;
        
        int ans = 0;
        int int_min = std::numeric_limits<int>::min();
        int int_max = std::numeric_limits<int>::max();

        for(int pos=0;end>=start && ans >= 0; end--,pos++){
            //使用std::pow方式基溢出（没有使用base *10这种方法的原因）
            ans += ((str[end]-'0') * std::pow(10,pos));
        }

        //溢出处理
         if(ans < 0){
                if(sign_index != -1 && str[sign_index]=='-')
                    return int_min;
                else
                    return int_max;
            }
        //正常情况
        if(sign_index != -1 && str[sign_index]=='-') ans = -ans;
        return ans;
    }
};
```