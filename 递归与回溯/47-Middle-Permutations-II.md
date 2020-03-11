# 47-Middle: Permutations II

## 考点

* 带剪枝的回溯

### 学到的东西

回溯问题一般都是用于所谓树型问题的搜索的，所以重要的是确定问题的树型到底长的什么样子。 __树型问题的每一个节点都是一个状态，其叶子节点往往还是问题的最终解之一。画出了树形图才知道该怎么去剪枝，怎么去写程序。__

## 题解

```cpp
//带剪枝的回溯
class Solution {
private:
    std::vector<int> local_ans;
    std::vector<std::vector<int>> ans;
    std::map<int,int> cons;
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        for(auto& num:nums)
             cons[num]++;
        DFSPrune(nums.size(),nums);
        return ans;
    }

    void DFSPrune(int n,std::vector<int>& nums){
        if(n==0) ans.push_back(local_ans);
        //用于控制同一层之间是否要剪枝
        std::map<int,bool> visited;
        for(auto& num:nums){
            if(visited.find(num)==visited.end()) visited[num]=false;
            if(visited[num]) continue;
            visited[num]=true;
            if(cons[num]>0){
                local_ans.push_back(num);
                cons[num]--;
                DFSPrune(n-1,nums);
                cons[num]++;
                local_ans.pop_back();
            }

        }
    }
};
```

