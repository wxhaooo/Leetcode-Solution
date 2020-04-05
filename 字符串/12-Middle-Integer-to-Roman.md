# 12-Middle:Integer-to-Roman

## 考点

## 题解

* 有技巧的字符串处理
* 贪心算法

### 优化的实现
```cpp
//速度比直接的实现快一倍左右
class Solution {
public:
     string intToRoman(int num) {
        int values[]={1000,900,500,400,100,90,50,40,10,9,5,4,1};
        string reps[]={"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
        
        string res;
        for(int i=0; i<13; i++){
            while(num>=values[i]){
                num -= values[i];
                res += reps[i];
            }
        }
        return res;
    }
};
```

### 直接的实现

```cpp
//把已知的先存起来，然后从最大的开始减，如果能减就一直减，不能就换成小的试试，效率比较低，需要92S
class Solution {
public:
    string intToRoman(int num) {
        std::string ans="";
        //如果把这条语句换成数组，速度其实比这个还慢三倍，因为初始化的时候要执行多次构造函数
        std::map<int,std::string> table;
        table[1]="I";table[4]="IV";table[5]="V";
        table[9]="IX";table[10]="X";table[40]="XL";
        table[50]="L";table[90]="XC";table[100]="C";
        table[400]="CD";table[500]="D";table[900]="CM";table[1000]="M";

        std::vector<int> vv{1000,900,500,400,100,90,50,40,10,9,5,4,1};

        int cur_ind=0;
        while(num){
            if(num-vv[cur_ind]>=0){num-=vv[cur_ind];ans+=table[vv[cur_ind]];}
            else cur_ind++;
        }
        return ans;
    }
};
```