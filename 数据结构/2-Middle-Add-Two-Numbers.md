# 2-Middle: Add Two Numbers

## 考点

* 链表的使用
* 进位模拟（大整数加法）

## 题解

```cpp
//非常直接的大整数加法实现
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* ll1 =l1,* ll2=l2;
        int aux = ll1->val + ll2->val;
        int carry = aux/10; aux%=10;
        ListNode* ans = new ListNode(aux);
        ListNode* ans_tmp = ans;
        ll1 = ll1->next;ll2 = ll2->next;

        while(ll1!=nullptr&&ll2!=nullptr){
            int ll1_val=ll1->val,ll2_val=ll2->val;
            aux = ll1_val + ll2_val + carry;
            carry=aux/10; aux%=10;
            ans_tmp->next = new ListNode(aux);
            ans_tmp = ans_tmp->next;
            ll1 = ll1->next;ll2=ll2->next;
        }

        while(ll1!=nullptr){
            aux = ll1->val+carry;
            carry = aux/10; aux%=10;
            ans_tmp->next = new ListNode(aux);
            ans_tmp = ans_tmp->next;
            ll1=ll1->next;
        }

         while(ll2!=nullptr){
            aux = ll2->val+carry;
            carry = aux/10; aux%=10;
            ans_tmp->next = new ListNode(aux);
            ans_tmp = ans_tmp->next;
            ll2=ll2->next;
        }

        if(carry) ans_tmp->next = new ListNode(1);

        return ans;
    }
};

```