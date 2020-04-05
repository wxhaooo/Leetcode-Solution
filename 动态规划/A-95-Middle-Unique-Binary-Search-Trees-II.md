# A-95-Middle: Unique Binary Search Trees II

## 考点

* 动态规划
* 递归与分治

> 这个题是`动态规划/96-Middle-Unique-Binary-Search-Trees`的变种，要求列出所有的二叉搜索树，难点在于分治的写法。 __关于分治的写法，首先要能把原问题拆成子问题，然后要知道怎么合并，这点是自己现在的薄弱点，需要多多练习才行。__

## 题解

```cpp
//分治+归并解决该问题
class Solution {
private:
    vector<TreeNode*> Build(int start,int end)
    {
        if(start>end) return vector<TreeNode*>(1,nullptr);

        vector<TreeNode*> res;
        for(int i=start;i<=end;i++){
            vector<TreeNode*> l = Build(start, i-1);
            vector<TreeNode*> r = Build(i+1,end);
            for(auto& ll:l){
                for(auto& rr:r){
                    TreeNode* node_ptr = new TreeNode(i);
                    node_ptr->left = ll;
                    node_ptr->right = rr;
                    res.push_back(node_ptr);
                }
            }
        }
        return res;
    }
public:
    vector<TreeNode*> generateTrees(int n) {
        if(n==0) return vector<TreeNode*>();
        std::vector<TreeNode*> res = Build(1,n);
        return res;
    }
};
```