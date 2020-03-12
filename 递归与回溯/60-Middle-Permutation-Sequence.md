# 60-Middle: Permutation Sequence

## 考点

* 回溯剪枝
> 这个剪枝需要观察到对于n个数构造的树，它的第i层子女每一个都有$(n-i)!$个子女，给每一个叶子编号，则有第一颗子树包含第$[1,(n-1)!]$个叶子，第二棵子树包含第$[(n-1)!+1,2(n-1)!-1]$个叶子。注意到解都是叶子，所以我们根据子树的叶子范围判断$k$在不在这个范围内，如果在这个范围内我们再深入这个子树重复上述操作，又注意到第$i$层的子树有$(n-i)!$个叶子，需要找的叶子也需要减去剪枝的叶子数。最后当$k==1$时，即要找的叶子就是这颗子树的第一个叶子时把所有没访问过的元素都加入即可。

## 题解

```cpp
//这个题解参考上面考点的解释
class Solution {
private:
    std::vector<bool> visited;
    std::string ans;
    int nn;
    std::vector<int> dp;
public:
    void Factorial(int n){
        dp.resize(n+1,0);
        dp[0]=dp[1]=1;
        for(int i=2;i<=n;i++){dp[i]=i*dp[i-1];}
    }

    string getPermutation(int n, int k) {
        visited.resize(n+1,false);
        Factorial(n);
        this->nn=n;
        DFSWithPrune(1,k);
        return ans;
    }

    //找到第k个叶子，回溯方法
    void DFSWithPrune(int level,int k){

        if(k==1) 
        {
            for(int i=1;i<=nn;i++){
                if(!visited[i]) ans+=('0'+i);
            } 
            return;
        }

        int low_range =1,high_range=dp[nn-level];
        for(int i=1;i<=nn;i++){
            if(visited[i]) continue;
            if(k>=low_range && k<=high_range){
                ans+=('0'+i);
                visited[i]=true;
                DFSWithPrune(level+1,k-low_range+1);
                return;
            }
            low_range+=dp[(nn-level)];
            high_range+=dp[(nn-level)];
        }
    }
};
```