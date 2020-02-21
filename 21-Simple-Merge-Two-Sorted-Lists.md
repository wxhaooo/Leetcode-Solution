# 21-Simple: Merge Two Sorted Lists

## 考点

* 链表的使用

## 题解

这题特别简单，注意处理链表为空的边界条件即可。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* ans,*cur;
        if(!l1 && !l2) return nullptr;
        if(l1 && !l2) return l1;
        if(!l1 && l2) return l2;
         
        if(l1->val < l2->val) {ans = l1; l1 = l1->next;}
        else {ans = l2; l2 = l2->next;}
        cur = ans;
        while(l1 && l2){
        if(l1->val < l2->val) {cur->next = l1; l1 = l1->next;}
        else {cur->next = l2; l2 = l2->next;}
        cur = cur->next;
        }
        while(l1){cur->next = l1; l1 = l1->next;cur = cur->next;}
        while(l2){cur->next = l2; l2 = l2->next;cur = cur->next;}
        return ans;
    }
};
```