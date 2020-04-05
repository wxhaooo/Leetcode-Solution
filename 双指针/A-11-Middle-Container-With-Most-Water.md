# A-11-Middle: Container With Most Water

## 考点

* 双指针法

## 题解

这道题最容易想到的方法是暴力解法，即算法复杂度为$O(n^2)$,但是实测超时了，当时虽然觉得是双指针，但是一直纠结怎么根据面积情况去移动指针，没想到比较高度来移动指针，最后就没有做出来。双指针这边的题做的有点少，要加强练习。(2020-03-26)

```cpp
//双指针方法，算法复杂度为O(n)
class Solution {
public:
    int maxArea(vector<int>& height) {
        int ans = -1;
        int i=0,j=height.size()-1;
        while(i<j){
            int h = std::min(height[i],height[j]);
            ans = max(ans,h*(j-i));
            height[i]<height[j] ? ++i:--j;
        }
        return ans;
    }
};
```