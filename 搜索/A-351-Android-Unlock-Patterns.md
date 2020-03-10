
# A-351-Middle: Android Unlock Patterns

## 考点

* 带约束的DFS

这个题的提示是DP，但是DP死活想不来，题解里竟然也没有好用的DP。但是DFS搜索是一下子就能想到的。__问题在于怎么处理那么不能访问的点__。一个想法是直接把约束建成图然后DFS，但是注意以下情况:当2被访问之后，再访问3，如果访问1这种情况是可以访问的。导致没法建图。这个时候自己就懵逼了。 __正确的思路应该是考虑待约束的DFS__。这种思路也很简单，就是在做DFS的时候直接丢掉不满足约束的结果即可， __问题就变成了怎么实现约束以及在DFS里检测约束。__

[这道题的约束检测方法见这里](https://baihuqian.github.io/2018-08-12-android-unlock-patterns/)

这个思路给了我们一个很好的启示，__要么想办法把约束存起来，要么要找到约束的规律来判断。__ 这个思路要学习。


### 学到的东西（搜索，递归，迭代，回溯与动态规划）

这道题的提示是动态规划，但题解里也没见到动态规划的解法。这几个概念还常常傻傻分不清。这里区分一下。

* 算法模式
> 所谓算法模式，就是使用相同的模式去设计不同的算法。而 __这个所谓的模式，正准确的说是搜索模式。__ 不同的模式使用的搜索策略和侧重不同。
> 枚举/穷举： __即一个一个解的列出来,不是一种算法模式，但是是所有算法的核心__
> 
> 分治：__把搜索空间一分为二分开搜索然后想办法合并。__
> 
> 贪心：__搜索空间存在某种最优路径，按照这种路径走可以更快的到达最优解。__
>
> 回溯: __解空间存在一定嵌套，可以将大问题拆分成小问题解决，而大问题和小问题的区别仅在于规模不同，可以使用同一解决方法解决。__
>
> 动态规划： __大问题可以拆分成小问题，而小问题间相互独立但是多个大问题会解决同一个小问题。所以可以通过存储小问题解的方式加速求解。__
> 
> 分支定界: __在满足约束条件的解中找出使某一目标函数值达到极大或极小的解，即在某种意义下的最优解__

所以总的来说，__算法基于穷举/枚举，算法模式是对穷举的优化，算法是算法模式的具体实现__

* 递归和迭代
> 递归和迭代是一种孪生的 __算法的实现方法__。比如BFS，DFS就分别可以用递归和迭代来实现。

* 搜索算法
> 按照算法模式设计出来解决特定问题的方法。比如DFS，BFS，二分查找等。

## 题解

```cpp
class Solution {
private:
    std::vector<bool> visited;
    std::vector<std::vector<int>> constraints;
public:
    int numberOfPatterns(int m, int n) {
        constraints.resize(10,std::vector<int>(10,0));
        visited.resize(10,false);

        constraints[1][3]=constraints[3][1]=2;
        constraints[1][7]=constraints[7][1]=4;
        constraints[1][9]=constraints[9][1]=5;
        constraints[2][8]=constraints[8][2]=5;
        constraints[3][7]=constraints[7][3]=5;
        constraints[3][9]=constraints[9][3]=6;
        constraints[4][6]=constraints[6][4]=5;
        constraints[7][9]=constraints[9][7]=8;

        int ans=0;

        ans+=SearchWithConstraints(1,0,m,n,0)*4;
        ans+=SearchWithConstraints(2,0,m,n,0)*4;
        ans+=SearchWithConstraints(5,0,m,n,0);

        return ans;
    }

    int SearchWithConstraints(int start,int len,int m,int n,int res)
    {
        len++;
        if(len>n) return res;
        if(len>=m) res++;
        visited[start]=true; 

        for(int i=1;i<10;i++){
            if(!visited[i]&&
            (constraints[start][i]==0||visited[constraints[start][i]])){
                res=SearchWithConstraints(i,len,m,n,res);
            }
        }

        visited[start]=false;
        return res;
    }
};
```