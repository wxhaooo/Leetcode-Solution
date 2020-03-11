# 39-Middle: Combination Sum

## 考点

* 带剪枝的回溯
> 这道题不剪枝没法过，会超时

### 学到的东西

这道题的剪枝不太好发现，所以需要学习。可以把这种问题当成一类问题的模板记忆一下。

## 题解

### 带剪枝的回溯

[参照这道题的题解](https://leetcode-cn.com/problems/combination-sum/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-2/)
```cpp
//带剪枝的回溯
class Solution {
private:
    std::vector<int> local_ans;
    std::vector<std::vector<int>> ans;
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        std::sort(candidates.begin(),candidates.end());
        DFSWithPrune(0,target,candidates);
        return ans;
    }

    void DFSWithPrune(int start,int target,vector<int>& candidates){
        if(target<0) return;

        if(target==0){
            ans.push_back(local_ans);
            return;
        }

        for(int i=start;i<candidates.size();i++){
            if(target-candidates[i]>=0){
                local_ans.push_back(candidates[i]);
                DFSWithPrune(i,target-candidates[i],candidates);  
                local_ans.pop_back(); 
            }  
        }
    }
};
```

### 不带剪枝的回溯

```cpp
//不带剪枝的回溯，结果超时
class Solution {
private:
    std::vector<int> local_ans;
    std::vector<std::vector<int>> ans;
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        DFSWithPrune(0,target,candidates);
        return ans;
    }

    void DFSWithPrune(int cur,int target,vector<int>& candidates){
        if(cur>target) return;

        if(cur==target){
            if(Valid(local_ans))
                ans.push_back(local_ans);
            return;
        }

        for(auto& candidate:candidates){
            local_ans.push_back(candidate);
            DFSWithPrune(cur+candidate,target,candidates);
            local_ans.pop_back();
        }
    }

    bool Valid(std::vector<int>& arr){
        for(auto& ans_e:ans){
            std::unordered_map<int,int> tmp;
            int count=0;
            for(auto& a:ans_e) {tmp[a]++;}
            for(auto& b:arr){tmp[b]--;}
            for(auto& entry:tmp) {if(entry.second==0) count++;}
            if(count==tmp.size()) return false;
        }
        return true;
    }
};
```

