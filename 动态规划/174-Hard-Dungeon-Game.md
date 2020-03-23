# 174-Hard: Dungeon Game

## 考点

* 二维反向动态规划
> 这道题的动态规划其实很好想，问题是不太容易编码。

## 题解

```cpp
//这个想一下DP就出来了，不太难
class Solution {
public:
	int calculateMinimumHP(vector<vector<int>>& dungeon) {
		if (dungeon.empty()) return 0;
		int n = dungeon.size(), m = dungeon[0].size();
		dungeon[n - 1][m - 1] = dungeon[n - 1][m - 1] > 0 ? 1 : 1 - dungeon[n - 1][m - 1];

		for (int i = n - 1; i >= 0; i--) {
			for (int j = m - 1; j >= 0; j--) {
				if (i == n - 1 && j == m - 1) continue;
				int tmp1, tmp2;
				tmp1 = tmp2 = std::numeric_limits<int>::max();
				if (dungeon[i][j] >= 0) {
					if (i + 1 < n) {
						tmp1 = dungeon[i + 1][j] - dungeon[i][j] > 0 ?
							dungeon[i + 1][j] - dungeon[i][j] : 1;
					}
					if (j + 1 < m) {
						tmp2 = dungeon[i][j + 1] - dungeon[i][j] > 0 ?
							dungeon[i][j + 1] - dungeon[i][j] : 1;
					}
				}
				else {
					if (i + 1 < n) {
						tmp1 = dungeon[i + 1][j] - dungeon[i][j];
					}

					if (j + 1 < m) {
						tmp2 = dungeon[i][j + 1] - dungeon[i][j];
					}
				}
				dungeon[i][j] = std::min(tmp1, tmp2);
			}
		}

		return dungeon[0][0];
	}
};
```