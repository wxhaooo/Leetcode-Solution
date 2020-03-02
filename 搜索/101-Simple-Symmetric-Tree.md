# 101-Simple: Symmetric Tree

## 考点

* 树的遍历（递归+迭代）

## 题解

```cpp
//这里还差一种迭代的方法去实现待补充
```

```cpp
//递归方法进行树的遍历判断对称性
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        if(!root->left && !root->right) return true;
        if(root->left && root->right &&
        root->left->val == root->right->val){
            return IsSymmetric(root->left,root->right);
        }
        return false;
    }

    bool IsSymmetric(TreeNode* left,TreeNode* right)
    {
        //如果都为nullptr也是对称
        if(!left && !right) return true;

        //这里是核心部分
        if(left && right &&
        left->val == right->val){
            //如果非常且当前这对节点对称，判断接下来的节点
            return IsSymmetric(left->left,right->right) &&
                    IsSymmetric(left->right, right->left);
        }

        return false;
    }
};
```