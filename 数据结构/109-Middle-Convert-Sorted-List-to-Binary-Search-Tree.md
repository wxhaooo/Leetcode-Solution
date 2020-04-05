# 109-Middle: Convert Sorted List to Binary Search Tree

## 考点

* 快慢指针找链表中点
> 做链表题的时候经常想不到快慢指针，__以后一看见链表题，如果和链表中点相关，要么链表有环，就要想到快慢指针。__（快慢指针可以总结出一些模板来使用，参见`/数据结构/快慢指针总结.md`）
* 二分搜索找中点
> 把链表元素直接放在vector里面二分搜索找中点即可

## 题解

### 快慢指针找中点

```cpp
//快慢指针找中点，并用prev记录slow的上一个节点，找到中点后断开与中点的链接
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if(head==nullptr) return nullptr;
        if(head->next==nullptr) return new TreeNode(head->val);
        
        ListNode* fast_ptr,* slow_ptr,* prev_ptr;
        slow_ptr = fast_ptr = prev_ptr = head;
        while(fast_ptr!=nullptr && fast_ptr->next!=nullptr){
            prev_ptr=slow_ptr;
            slow_ptr=slow_ptr->next;
            fast_ptr=fast_ptr->next->next;
        }
        prev_ptr->next=nullptr;
        TreeNode* root = new TreeNode(slow_ptr->val);
        root->left = sortedListToBST(head);
        root->right = sortedListToBST(slow_ptr->next);
        return root;
    }
};
```

### 二分搜索找中点

```cpp
//二分搜索找中点的方式建立平衡二叉搜索树，注意平衡二叉搜索树不止一种，任意一种均可
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        std::vector<int> arr;
        while(head!=nullptr){arr.push_back(head->val);head=head->next;}

        return ToBST(0,arr.size()-1,arr);
    }

    TreeNode* ToBST(int start,int end,std::vector<int>& arr){
        if(start>end) return nullptr;
        int mid = start+(end-start)/2;

        TreeNode* root=new TreeNode(arr[mid]);
        root->left = ToBST(start,mid-1,arr);
        root->right = ToBST(mid+1,end,arr);

        return root;
    }
};
```