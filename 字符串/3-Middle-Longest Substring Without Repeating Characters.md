# 3-Simple: Longest Substring Without Repeating Characters

## 考点

* 双指针法（滑动窗法）

## 题解
```cpp
//按照上面的思路，其实是遇到重复的元素时，希望前面的索引能直接移到
//重复索引的下一个位置，即记录元素的位置即可
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        std::array<int,257> table{-1};
        int max_len = 0;
        int i = 0,j = 0;
        for(;j<s.size();j++){
            //如果当前字符重复了（索引不为-1了）
            if(table[s[j]-' '] >0){
                //把i直接移动到重复位置的下一位
                i = std::max(i,table[s[j]-' ']);
            }
            //长度计算
            max_len = std::max(max_len,j-i+1);
            //j的位置直接记为自己的下一位即可
            table[s[j]-' '] = j+1;
        }
        return max_len;
    }
};
```

```cpp
//代码更精简的滑动窗法,思路和下面一条一样
//时间：8ms
//空间：9.3MB
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        std::array<int,257> table{0};
        int i = 0,j = 0;
        int max_len = 0;
        for(;i<s.size() && j<s.size();){
            if(table[s[j]-' '] <= 0)
            {
                table[s[j++]-' ']++;
                max_len = std::max(max_len,j- i);
            }
            else
                table[s[i++]-' ']--; 
        }
       return max_len;
    }
};
```

```cpp
//一种滑动窗法
//时间: 20ms左右
//空间：9.4MB
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        std::array<int,257> table{0};
        int i = 0,j = 0;
        int max_len = 0;
        for(;i<s.size() && j<s.size();){
            //如果j处的字符没有出现过，那就计算现在的窗口大小然而扩充窗口
            if(table[s[j]-' '] <= 0)
            {
                table[s[j]-' ']++;
                max_len = std::max(max_len,j- i + 1);
                j++;
            }
            else
            {
                //否则把窗口向前移动一个位置，等于缩小窗口，且下一次还是缩小窗口，直到缩到不存在重复字符的位置为止，然后从这个字符再开始找新的窗口
                table[s[i]-' ']--; 
                i++; 
                continue;
            }
            
        }
       return max_len;
    }
};
```

```cpp
//比较Low的暴力解法，算法复杂度O(n^2)也能过，不过主要学习双指针法
//时间：84ms
//空间: 9.4MB
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        
        if(s.size() == 0) return 0;
        if(s.size() == 1) return 1;

        int ans = 1;
        int start = 0,cur = 1;

        std::array<int,257> table{0};
        table[s[start]-' ']++;
        for(;cur <s.size();){
            if(table[s[cur]-' '] > 0){
                if(cur - start > ans)
                    ans = cur - start;
                start++;cur = start + 1;
                table.fill(0);table[s[start]-' ']++;
                continue;
            }
            table[s[cur]-' ']++;
            cur++;
        }
        if(table[s[cur - 1]-' '] <= 1 && cur - start > ans)
            ans = cur - start;
        return ans;
    }
};
```