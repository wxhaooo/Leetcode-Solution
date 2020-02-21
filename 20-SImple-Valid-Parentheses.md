# 20-Simple: Valid Parentheses

## 考点

* 后序遍历
* 栈与字符串

## 题解

这道题的巧妙解法很多，参见这道题的leetcode上的题解

```cpp
//简单的方案，直接括号匹配
//用了map构造的哈希表，所以时间和下边差不多
class Solution {
public:
    bool isValid(string s) {

       std::stack<char> tmp;
       std::map<char,char> hash_table;
       hash_table['('] = ')';
       hash_table['['] = ']';
       hash_table['{'] = '}';

       for(int i=0;i<s.size();i++){
           if(hash_table.find(s[i])!=hash_table.end())
                tmp.push(s[i]);
            else if(tmp.empty()||hash_table[tmp.top()]!=s[i])
            return false;
            else
                tmp.pop();
       }
       return tmp.empty();
    }
};
```


```cpp
//复杂版的解决方法，因为没完全get到题意，题目的意思其实就是编译原理的括号匹配问题
class Solution {
public:
    bool isValid(string s) {

        if(s.size() == 0)
            return true;

        std::stack<char> tmp;
        std::reverse(s.begin(),s.end());
        tmp.push(s.back());
        s.pop_back();

        while(!tmp.empty() || s.size() > 0){
            if(s.size()==0) return false;
            char cur = s.back();
            if(tmp.empty() &&
            (cur == ')' || cur == '}' || cur ==']') )
            return false;
            if((cur ==')' && tmp.top()!='(') ||
                (cur =='}' && tmp.top()!='{') ||
                (cur == ']' && tmp.top() !='['))
                return false;
            if(cur == '(' || cur =='[' || cur=='{')
                tmp.push(cur);
            else
                tmp.pop();
            s.pop_back();
        }

        return tmp.empty();
    }
};
```