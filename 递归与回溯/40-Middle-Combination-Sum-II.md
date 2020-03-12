# 40-Middle: Combination Sum II

## 考点

* 带剪枝的回溯

> 自己的写法并不简洁，效率也不高，多学习学习别人的写法。

### 学到的东西

39,40这两道题的剪枝方法不仅要记忆，还要掌握。__一种是对重复元素访问的模板，一种是防止水平方向的模板。__

## 题解

## 更好些的剪枝

[这种剪枝方法参考这篇题解](https://leetcode-cn.com/problems/combination-sum-ii/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-m-3/)

```cpp
class Solution {
private:
    std::vector<int> local_ans;
    std::vector<std::vector<int>> ans;
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        if(target==0||candidates.empty()) return ans;
        std::sort(candidates.begin(),candidates.end());
        DFSWithPrune(0,target,candidates);
        return ans;
    }

    void DFSWithPrune(int start,int target,vector<int>& candidates)
    {
        if(target<0) return;

        if(target==0) {ans.push_back(local_ans); return;}

        for(int i=start;i<candidates.size();i++){
            //这行代码是这中剪枝方法的关键，只防止树的同一层重复，不同层可以使用相同元素
            if(i>start && candidates[i]==candidates[i-1]) continue;
            local_ans.push_back(candidates[i]);
            DFSWithPrune(i+1,target-candidates[i],candidates);
            local_ans.pop_back();
        }
    }
};
```

## 不太优美的剪枝

```cpp
class Solution {
private:
    //table用于控制每个元素仅访问一次
    std::map<int,int> table;
    std::vector<int> local_ans;
    std::vector<std::vector<int>> ans;
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        if(target==0||candidates.empty()) return ans;
        for(auto& candidate:candidates) table[candidate]++;
        //排序如题39的方法，目的是为了剪除重复的元素
        std::sort(candidates.begin(),candidates.end());
        candidates.erase(std::unique(candidates.begin(),candidates.end()),candidates.end());
        DFSWithPrune(0,target,candidates);
        return ans;
    }

    void DFSWithPrune(int start,int target,vector<int>& candidates)
    {
        if(target<0) return;

        if(target==0) {ans.push_back(local_ans); return;}

        for(int i=start;i<candidates.size();i++){
            if(table[candidates[i]]>0){
            table[candidates[i]]--;
            local_ans.push_back(candidates[i]);
            DFSWithPrune(i,target-candidates[i],candidates);
            local_ans.pop_back();
            table[candidates[i]]++;
            }
        }
    }
};
```