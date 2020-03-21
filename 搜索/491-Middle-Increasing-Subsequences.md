# 491-Middle: Increasing-Subsequences

## 考点

* 搜索+剪枝

### 学到的东西

搜索和剪枝的关键就在于画出决策树，然后根据到底是横向不能出现重复还是纵向不能出现重复来决定怎么剪枝。 __剪枝方法无非横向剪枝用局部的visisted数组或者set，纵向剪枝用全局的visited数组和set。__

## 题解

```cpp
//标准的DFS
class Solution {
    private:
        std::vector<int> local_ans;
        std::vector<std::vector<int>> ans;
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        DFS(0,nums);
        return ans;
    }

    void DFS(int start,std::vector<int>& nums){

        if(local_ans.size()>=2){ans.push_back(local_ans);}

        std::unordered_set<int> visited;

        for(int i=start;i<nums.size();i++){
            if(visited.find(nums[i])==visited.end()
            &&(local_ans.empty()||nums[i]>=local_ans.back())){
                visited.insert(nums[i]);
                local_ans.push_back(nums[i]);
                DFS(i+1,nums);
                local_ans.pop_back();
            }
        }
    }
};
```