# 365-Middle:Water-and-Jug-Problem

## 考点

* 多变量状态搜索
* 数学知识(贝祖定理)
> $ax+by=z$ 有解当且仅当 $z$ 是 $x,y$ 的最大公约数的倍数。因此我们只需要找到$x,y$的最大公约数并判断$z$是否是它的倍数即可。

### 学到的东西

虽然一眼可以看出来这是个搜索问题，但是 __要注意到这里搜索的状态是两个水壶的装水量，是多状态的。__而且 __致命的是如果直接使用数组来存储状态是否被访问过则因为空间太大导致内存爆炸！（2G以上内存）__ 所以就必须自定义哈希表或者map的比较函数，这点要学会。

__关于std::pair<int,int>的哈希函数记忆一下，用的比较多__

另外那个数学方法太巧妙了，也掌握一下。

数学方法参见：[数学方法解决这道题](https://leetcode-cn.com/problems/water-and-jug-problem/solution/shui-hu-wen-ti-by-leetcode-solution/)

## 题解

### 数学方法解决该问题

```cpp
//熟练背诵gcd的写法，另外也注意下最小公倍数的求法
int gcd(int a,int b){
    if(b==0) return a;
    return gcd(b,a%b);
}

class Solution {
public:
	bool canMeasureWater(int x, int y, int z) {
		if(z>x+y) return false;
         if (x == 0 || y == 0)
            return z == 0 || x + y == z;
        return z%gcd(x,y)==0;
	}
};
```

### 搜索解决该问题

```cpp
//DFS和BFS都可以，难点在于状态的定义和编码上
class HashFunc
{
public:
	std::size_t operator()(const std::pair<int, int>& p) const {
		return std::hash<int>()(p.first) ^ std::hash<int>()(p.second);
	}
};

class Solution {
	int x, y;
public:
	bool canMeasureWater(int x, int y, int z) {
		// if(z>x+y) return false;
		this->x = x;
		this->y = y;
		return DFS(0, 0, z);
	}

	bool DFS(int xx, int yy, int target) {
		std::unordered_set<std::pair<int, int>,HashFunc> visited;
		// std::vector<std::vector<bool>> visited(x+1,std::vector<bool>(y+1,false));
		std::stack<std::pair<int, int>> st;
		st.push(std::make_pair(xx, yy));
		visited.insert(std::make_pair(xx, yy));
		while (!st.empty()) {
			auto cur = st.top(); st.pop();
			int cur_x = cur.first, cur_y = cur.second;
			if (cur_x == target || cur_y == target || cur_x + cur_y == target) return true;

			//倒空
			if (visited.find(std::pair<int, int>(0, cur_y)) == visited.end())
			{
				visited.insert(std::pair<int, int>(0, cur_y));
				st.push(std::make_pair(0, cur_y));
			}
			if (visited.find(std::pair<int, int>(cur_x, 0)) == visited.end())
			{
				visited.insert(std::pair<int, int>(cur_x, 0));
				st.push(std::make_pair(cur_x, 0));
			}

			//倒满
			if (visited.find(std::pair<int, int>(x, cur_y)) == visited.end())
			{
				visited.insert(std::pair<int, int>(x, cur_y));
				st.push(std::make_pair(x, cur_y));
			}
			if (visited.find(std::pair<int, int>(cur_x, y)) == visited.end())
			{
				visited.insert(std::pair<int, int>(cur_x, y));
				st.push(std::make_pair(cur_x, y));
			}

			//互相倒
			if (cur_y < y) {
				int new_y = std::min(cur_x + cur_y, y);
				int new_x = (new_y == (cur_x + cur_y) ? 0 : (cur_x - (new_y - cur_y)));
				if (visited.find(std::pair<int, int>(new_x, new_y)) == visited.end()) {
					visited.insert(std::pair<int, int>(new_x, new_y));
					st.push(std::make_pair(new_x, new_y));
				}
			}
			if (cur_x < x) {
				int new_x = std::min(cur_x + cur_y, x);
				int new_y = (new_x == (cur_x + cur_y) ? 0 : (cur_y - (new_x - cur_x)));
				if (visited.find(std::pair<int, int>(new_x, new_y)) == visited.end()) {
					visited.insert(std::pair<int, int>(new_x, new_y));
					st.push(std::make_pair(new_x, new_y));
				}
			}

		}
		return false;
	}
};
```
