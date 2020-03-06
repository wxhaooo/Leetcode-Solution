# 面试题57-SImple:II. 和为s的连续正数序列

## 考点

* 数学
* 滑动窗

## 题解

[数学解法参考自己的这个题解](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/shu-xue-fang-fa-jie-jue-gai-wen-ti-c-by-wxntech/)

```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        std::vector<std::vector<int>> ans;
        std::vector<int> local_ans;
        for(int s=1;s<target/2 + 1;s++){
            auto tmp = 1 - 2*s + std::sqrt(std::pow(2*s-1,2)+8*target);
            tmp/=2;
            if(tmp==(int)tmp){
                for(int i=0;i<tmp;i++){
                    local_ans.push_back(s+i);
                }
                ans.push_back(local_ans);
                local_ans.clear();
            }
        }
        return ans;
    }
};
```