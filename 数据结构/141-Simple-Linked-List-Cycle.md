# 141-Simple: Linked List Cycle

## 考点

* 链表判环（快慢指针法）


## 题解

```cpp

class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(!head ||!head->next) return false;
        ListNode* fast_ptr = head, *slow_ptr = head;
        do
        {
            slow_ptr = slow_ptr->next;
            fast_ptr = fast_ptr->next;
            if(!fast_ptr) break;
            fast_ptr = fast_ptr->next;
        //有环时最终一定会在环中某个节点相遇，所以一定会有
        slow_ptr==fast_ptr，则退出循环
        }while(slow_ptr && fast_ptr && slow_ptr!=fast_ptr);

        //没有环时一定是快指针先走到了头，慢指针还在链表里面
        if(!fast_ptr && slow_ptr) return false;

        return true;
    }
};
```