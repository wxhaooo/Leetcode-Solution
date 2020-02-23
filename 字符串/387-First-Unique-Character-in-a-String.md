# 387-Simple: First Unique Character in a String

## 考点

* hash表和字符串匹配

## 题解

```cpp
//这种方法分支太多，速度很慢
//72 ms,13.4 MB
class Solution {
public:
    int firstUniqChar(string s) {
        if(s.size()==0) return -1;
        int ans_index = std::numeric_limits<int>::max();
        std::unordered_map<char,std::pair<int,int>> hash_table;
        for(int i=0;i<s.size();i++){
            hash_table[s[i]].first++;
            hash_table[s[i]].second = i; 
        }
        for(auto& hash_entry:hash_table){
            if(hash_entry.second.first == 1 && hash_entry.second.second < ans_index)
                ans_index = hash_entry.second.second;
        }
        if(ans_index < std::numeric_limits<int>::max())
            return ans_index;
        return -1;
    }
};
```

```cpp
//速度稍快一些，减少了分支，但是因为用了std::unordered_map
//速度和内存的开销还是比较高
//64 ms,13.2 MB
class Solution {
public:
    int firstUniqChar(string s) {
        if(s.size()==0) return -1;
        std::unordered_map<char,int> hash_table;
        for(auto& ch:s)
            hash_table[ch]++;
        for(int i=0;i<s.size();i++)
            if(hash_table[s[i]] == 1) return i;
      return -1;
    }
};
```

```cpp
//换成数组，速度增快很多，内存也减少一些
//24 ms,13MB
class Solution {
public:
    int firstUniqChar(string s) {
        if(s.size()==0) return -1;
        std::array<int,26> hash_table{0};
        for(auto& ch:s)
            hash_table[ch-'a']++;
        for(int i=0;i<s.size();i++)
            if(hash_table[s[i]-'a'] == 1) return i;
      return -1;
    }
};
```



