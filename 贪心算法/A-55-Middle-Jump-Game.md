# 55-Middle: Jump Game

## 考点

* 贪心算法
> 这个算法的思路还是比较巧妙的

## 学到的东西

这个可以看成一个出发到达问题，仔细观察会发现一个点可以作为多个点的到达，但却是唯一的出发点。从出发点思考比从到达点思考要方便些。

## 题解

```cpp
//使用贪心的正确解法，考虑的是从当前点出发能走到的最远距离，如果有某个点的当前大于前面记录的最大距离了，那说明到不了这个点，返回false,否则一直能走下来，就返回true
class Solution {
public:
    bool canJump(vector<int>& nums) {
       int k=0;
       for(int i=0;i<nums.size();i++){
           if(i>k) return false;
           k = std::max(k,nums[i]+i);
           //如果最大距离都超过数组长度了，就可以提前跳出
            if(k>=nums.size()-1) return true;
       } 
       return true;
    }
};
```

```cpp
//错误解法，试图去贪从当前点到下一点再到下一点的最远距离，问题出在这种方法立马会转移到下一个点
class Solution {
public:
    bool canJump(vector<int>& nums) {
        if(nums.size()==0) return true;
        int cur_pos = 0;int cur_max_step = nums[0];
        while(cur_pos<nums.size()-1){
            int local_max_step = 0,next_pos=0;
            if(cur_pos + cur_max_step >=nums.size()-1) return true;
            for(int i=1;i<=cur_max_step;i++){
                if(nums[cur_pos+i]>=local_max_step){
                local_max_step = nums[cur_pos+i];
                next_pos = cur_pos + i;
                }
            }
            if(local_max_step >= nums[cur_pos]){
                cur_pos = next_pos;
                cur_max_step = local_max_step;
            }
            else{
                cur_pos = cur_pos + nums[cur_pos];
                cur_max_step = nums[cur_pos];
            }
            if(cur_max_step==0) return false;
        }

        return true;
    }
};
```