# 1103-Simple: Distribute Candies to People

## 考点

* 数组
* 数学问题

## 题解

```cpp
//直接按要求来分就行
class Solution {
public:
    vector<int> distributeCandies(int candies, int num_people) {
        std::vector<int> ans(num_people,0);
        int cur = 0;
        int i=1;
        while(candies>0)
        {
            if(i<candies)
                ans[cur] += i;
            else{
                ans[cur]+=candies;
                break;
            }
            cur++;
            cur %=num_people;
            candies-=i;
            i++;
        }
        return ans;
    }
};
```