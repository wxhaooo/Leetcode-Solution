# 51-Hard: N-Queens

## 思路

* 回溯加剪枝优化
> 这是第一道自己想出来还一次AC的hard题目，所以撒花记录一下。

## 题解

### 第二版代码

[整体思路见这个题解](https://leetcode-cn.com/problems/n-queens/solution/hui-su-suan-fa-xiang-jie-by-labuladong/)

```cpp
//状态是第i行，试图选择某列放皇后，通过约束决定到底放哪合适，速度是第一版的40倍以上
class Solution {
    vector<vector<string>> ans;
    vector<string> local_ans;
    vector<bool> row,col,diag,opp_diag;
    int n;
public:
    vector<vector<string>> solveNQueens(int n) {
        if(n==0) return ans;
        this->n = n;
        row.resize(n,false);col.resize(n,false);diag.resize(2*n-1,false);
        opp_diag.resize(2*n-1,false); 
        local_ans = vector<string>(n,string(n,'.'));

        DFSWithPrune(0);

        return ans;
    }

    void DFSWithPrune(int i)
    {
        if(i==n){
            ans.push_back(local_ans);
            return;
        }

        for(int j=0;j<n;j++){
            if(col[j]) continue;
            if(diag[j-i+n-1]||opp_diag[j+i]) continue;
            local_ans[i][j]='Q';
            row[i]=col[j]=diag[j-i+n-1]=opp_diag[j+i]=true;
            DFSWithPrune(i+1);
            local_ans[i][j]='.';
            row[i]=col[j]=diag[j-i+n-1]=opp_diag[j+i]=false;
        }
    }
};
```

### 第一版代码
```cpp
//通过但是时间比较长，以皇后数作为状态变量，试图在整个棋盘上摆放皇后，用row,col,diag,opp_diag分别判断该行、列，（副）对角线，逆（副）对角线上是否已经有元素

//对于位置(i,j)，它在第j-i条对角线上，在i+j-n条副对角线上，因为这两个值都可能为负数，所以需要加上n-1或者n把它映射到正数上，所以有位置(i,j)，它在第j-i+n-1条对角线上，在i+j条副对角线上
class Solution {
    vector<vector<string>> ans;
    vector<string> local_ans;
    vector<bool> row,col,diag,opp_diag;
    int n;
public:
    vector<vector<string>> solveNQueens(int n) {
        if(n==0) return ans;
        this->n = n;
        row.resize(n,false);col.resize(n,false);diag.resize(2*n-1,false);
        opp_diag.resize(2*n-1,false); 
        local_ans = vector<string>(n,string(n,'.'));

        DFSWithPrune(0,0,n);

        return ans;
    }

    void DFSWithPrune(int start_x,int start_y,int now)
    {
        if(now==0){
            ans.push_back(local_ans);
            return;
        }

        for(int i=start_x;i<n;i++){
            if(row[i]) continue;
            for(int j=start_y;j<n;j++){
                if(col[j]) continue;
                //对于副对角线的判断见上面的注释
                if(diag[j-i+n-1]||opp_diag[j+i]) continue;
                local_ans[i][j]='Q';
                row[i]=col[j]=diag[j-i+n-1]=opp_diag[j+i]=true;
                DFSWithPrune(i+1,0,now-1);
                local_ans[i][j]='.';
                row[i]=col[j]=diag[j-i+n-1]=opp_diag[j+i]=false;
            }
        }
    }
};
```