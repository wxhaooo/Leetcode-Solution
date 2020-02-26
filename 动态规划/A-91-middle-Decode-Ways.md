# 91-Middle-Decode-Ways

## 考点

* 动态规划
> 动态规划的点不容易想到，先理解并背过

## 题解

```cpp
class Solution {
public:
    int numDecodings(string s) {
        if(s.size() == 0) return 0;
        //首个字符不能为0，不合法
        if(s[0]=='0') return 0;

        std::array<int,10005> d{0};
        d[0] =1;
        for(int i=1;i<s.size();i++){
            //数字为0
            if(s[i]=='0'){
                if(s[i-1]=='1' || s[i-1]=='2'){
                if(i==1)
                    d[i] = 1;
                else
                    d[i] = d[i-2];
                }
                else
                    return 0;
            }
            //数字不为0，但s[i-1]为1时怎么都合法
            else if(s[i - 1]=='1'){
                if(i == 1)
                    d[1] = d[0] + 1;
                else 
                    d[i] = d[i-1] + d[i-2];
            }
            //数字不为0,但s[i-2]为2时有一部分合法
            else if(s[i - 1]=='2' && s[i]>='0' && s[i]<='6'){
                if(i==1)
                    d[i] = d[i-1] + 1;
                else
                    d[i] = d[i-1] + d[i-2];
            }
            //其他情况，只能做单个解读，不增加解码顺序
            else
                d[i] = d[i-1];
        }

        return d[s.size() -1];
    }
};
```