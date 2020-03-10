# 46-Middle: Permutations

## 考点

* 回溯
> [这个考点总结不错，仔细看看](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)


## 题解

```cpp
//这个全排列的回溯非常好想，但是一直实现不正确，原因就出在注释那部分
class Solution {
public:
    std::vector<bool> visited;
    vector<int> local_ans;
    std::vector<std::vector<int>> ans;
    int n;
public:
    vector<vector<int>> permute(vector<int>& nums) 
    {
        n=nums.size();
        visited.resize(n,false);
        permute(n,nums);

        for(auto& vec: ans){
        for(auto& v:vec){
            v = nums[v];
        }
    }

        return ans;
    }

    void permute(int m, vector<int>& nums)
    {
        if(m==0){
            //这里不能这么用，因为执行后local_ans被改变了，这个bug还比较隐蔽
            // for(int i=0;i<local_ans.size();i++){
            //     // local_ans[i]=nums[local_ans[i]];
            // }
            ans.push_back(local_ans);
            return;
        }

        for(int i=0;i<n;i++){
            if(!visited[i]){
                visited[i]=true;
                local_ans.push_back(i); 
                permute(m-1,nums);
                local_ans.pop_back();
                visited[i]=false;
            }
        }
    }
};
```