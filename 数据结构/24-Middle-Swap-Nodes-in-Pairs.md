# 24-Middle: Swap Nodes in Pairs

## 考点

* 链表的使用和结构

## 题解

```cpp
//先翻转头部，再翻转后面的，注意这里是一次翻转一对，如果有剩余就不翻转
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head==nullptr||head->next==nullptr) return head;
        ListNode* aux = head->next->next;
        head->next->next = head;
        head = head->next;
        head->next->next = aux;
        ListNode* prev_node=head->next;
        ListNode* cur_node=prev_node->next;
        while(cur_node!=nullptr&&cur_node->next!=nullptr){
            ListNode* next_node=cur_node->next;
            ListNode* tmp = next_node->next;
            prev_node->next=next_node;
            next_node->next=cur_node;
            cur_node->next=tmp;
            prev_node=cur_node;
            cur_node=cur_node->next;
        }

        return head;
    }
};
```