# 13-Simple: Roman to integer

## 考点

* 简单的字符串转换
* 哈希表
> 注意区分C++中有序hash(map)和无序hash(unorder_map)的区别，__如果对于元素没有排序要求只做查询的话用unorder_map，效率更高，反之用map.__

## 题解

```cpp
//题目比较简单，有思路的话注意好实现即可，一次过
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char,int> hash_table;
        hash_table['I'] = 1; hash_table['V'] = 5;
        hash_table['X'] = 10; hash_table['L'] = 50;
        hash_table['C'] = 100; hash_table['D'] = 500;
        hash_table['M'] = 1000; 

        int cur_pos = 0,start_pos = 0;
        int ans = 0;
        while(start_pos < s.size()){
            int next_pos = cur_pos + 1;
            if(hash_table[s[next_pos]] > hash_table[s[cur_pos]])
            {
                while(start_pos<cur_pos)
                {
                    ans+=(hash_table[s[start_pos]]);
                    start_pos++;
                }
                ans +=(hash_table[s[next_pos]]-hash_table[s[cur_pos]]);
                cur_pos +=2;
                start_pos = cur_pos;
                continue;
            }
            cur_pos++;
            if(cur_pos>=s.size())
            {
                while(start_pos<cur_pos)
                {
                    ans+=(hash_table[s[start_pos]]);
                    start_pos++;
                }
            }
        }
        return ans;
    }
};
```