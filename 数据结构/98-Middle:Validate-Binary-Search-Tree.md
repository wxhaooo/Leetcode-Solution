# 98-Middle: Validate Binary Search Tree

## 考点

* 树的遍历（先序，中序，后序）
* 二叉搜索树的性质

## 题解

```cpp
//题目比较简单，就不写详细题解了，按中序排列出来然后判断是否满足二叉搜索树的性质即可
class Solution {
private:
    std::vector<int> data;
public:
    bool isValidBST(TreeNode* root) {
        if(root==nullptr || (root->left==nullptr && root->right==nullptr)) return true;
        InorderTraversal(root);

       for(int i=1;i<data.size();i++){
           if(data[i]<=data[i-1])
           return false;
       } 
       return true;
    }

    void InorderTraversal(TreeNode* root)
    {
        if(root==nullptr) return;

        InorderTraversal(root->left);
        data.push_back(root->val);
        InorderTraversal(root->right);
    }
};
```