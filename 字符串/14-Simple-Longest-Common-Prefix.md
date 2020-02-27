14-Simple: Longest Common Prefix

## 考点

* 多个字符串找最长前缀


## 题解

最优解法是借助trie字典树。将这些字符串存储到trie树中。那么trie树的第一个分叉口之前的单分支树的就是所求。没有在这里实现。


```cpp
//先找到最短序列的长度（因为最长前缀最多就是最短序列的长度）然后逐个比较，速度比较慢，内存也用的比较多，也要注意边界条件的处理（输入为空等）
//复杂度O(m*n)
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) 
    {
        std::string ans = "";
        int min_str_length = std::numeric_limits<int>::max();
        if(strs.size()==0)
            return ans;

        for(auto& str:strs)
        {
            if(str.size()<min_str_length)
            min_str_length = str.size();
        }

        for(int i=0;i<min_str_length;i++)
        {
            int j =0;
           for(;j < strs.size() - 1 && strs[j][i] == strs[j + 1][i];j++){}
            if(j==strs.size() - 1)
                break;
            ans += strs[j][i];
        //写成下面这样速度比上面慢三倍！！！，因为多一个分支，而且这个分支还经常需要判断
        //    if(j==strs.size() - 1)
        //         ans += strs[j][i];
        //     else
        //         break;
        }
        return ans;
    }
};
```

```cpp
//使用hash表超时，因为多余了不断对哈希表的遍历
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        std::string ans = "";
        std::unordered_map<char,int> hash_table;
        int num_of_str = strs.size();
        int min_str_length = std::numeric_limits<int>::max();
        for(auto& str:strs)
        {
            if(str.size()<min_str_length)
            min_str_length = str.size();
        }
        for(int i=0;i<min_str_length;i++)
        {
            for(auto& str:strs){
                hash_table[str[i]]++;
            }
            for(auto& table_entry:hash_table){
                if(table_entry.second == num_of_str){
                    ans += table_entry.first;
                    break;
                }
            }
            hash_table.clear();
        }
        return ans;
    }
};
```