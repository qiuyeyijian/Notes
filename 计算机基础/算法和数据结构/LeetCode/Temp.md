# LCP 38 守卫城堡

首先要处理一下瞬移点，可能的情况有两种：允许恶魔走到瞬移点和不允许恶魔走到瞬移点。这两种情况等价于将所有瞬移点当做恶魔和当所有瞬移点当做恶魔。

对于网格内的点，主要有四种情况，可以用数字进行编码：

| 状态                                                         | 编码 |
| ------------------------------------------------------------ | ---- |
| 空地                                                         | 0    |
| 城堡或城堡可到达的地方（把城堡当作可移动的对象,相当于恶魔可以从这个地方到达城堡） | 1    |
| 恶魔或恶魔可达到的地方                                       | 2    |
| 障碍物                                                       | 3    |



## DP实现

### 状态表示

第$i-1$列第一、二行状态分别为$s_1, s_2$时，放置的最小障碍数
$$
f(i, s_1, s_2)
$$

### 状态计算

由第$i-1$列队第i列造成的影响表示为$t_1, t_2$，在第$i$列放置$cost$个障碍物，得到状态转移方程为：
$$
f(i, t_1, t_2) = min(f[i][t_1][t_2], f(i-1, s_1, s_2)+cost)
$$

### 初始状态

在第1列前面一列放置空地，此时不放置障碍物，因为前面是边界外
$$
f[0][0][0] = 0
$$

### 目标状态

$$
min\{f[n][s1][s2] | 0 \le s_1 \le 3, 0 \le s_2 \le 3\}
$$

### 时间复杂度

$$
O(4^2 \times n)
$$



### 代码

```cpp
class Solution {
 public:
  const int INF = 65535;
  int n, m;
  int f[10010][4][4];
  const int dx[4] = {0, 0, 1, -1}, dy[4] = {1, -1, 0, 0};
  unordered_map<char, int> mp = {{'.', 0}, {'C', 1}, {'S', 2}, {'#', 3}};

  // 将grid网格内所有字符'p',变成指定字符ch
  vector<string> change(const vector<string>& grid, char ch) {
    vector<string> ans = grid;
    for (auto& x : ans) {
      std::replace(x.begin(), x.end(), 'P', ch);
    }
    return ans;
  }

  // 检查城堡是否在恶魔的旁边
  bool check(vector<string>& grid) {
    // 遍历网格内所有的点
    for (int i = 0; i < m; ++i) {
      for (int j = 0; j < n; ++j) {
        // 如果当前位置是恶魔，下一个位置是城堡，则返回false
        if (grid[i][j] == 'S') {
          for (int k = 0; k < 4; ++k) {
            int x = i + dx[k], y = j + dy[k];
            // [x,y]在边界内才能继续下一步的判断
            if (x >= 0 && x < m && y >= 0 && y < n) {
              if (grid[x][y] == 'C') return false;
            }
          }
        }
      }
    }
    return true;
  }

  void update(int i, int s1, int s2, int t1, int t2, int cost) {
    // 如果s1是城堡或者恶魔可到达的地点
    if (s1 == 1 || s1 == 2) {
      // t1刚好是恶魔或者城堡可到达的地点，则说明无解
      if (s1 + t1 == 3) return;

      // 如果t1是空地，则说明s1可以走到t1，从而更新t1
      if (t1 == 0) t1 = s1;
    }

    if (s2 == 1 || s2 == 2) {
      if (s2 + t2 == 3) return;
      if (t2 == 0) t2 = s2;
    }

    // 如果t1是城堡可到达的地点，t2是恶魔可到达的地点，则说明无解，反之亦然
    if ((t1 == 1 || t1 == 2) && t1 + t2 == 3) return;
    // 如果t1是城堡或者恶魔可到达的地点，t2是空地，则t2可以被t1更新
    if ((t1 == 1 || t1 == 2) && t2 == 0) t2 = t1;
    // 同理，如果t2是城堡或者恶魔可到达的地点，t1是空地，则t1可以被t2更新
    if ((t2 == 1 || t2 == 2) && t1 == 0) t1 = t2;

    // 要么维持现状，要么在放下num个障碍物
    f[i][t1][t2] = std::min(f[i][t1][t2], f[i - 1][s1][s2] + cost);
  };

  int cal(vector<string>& grid) {
    if (!check(grid)) return INF;

    memset(f, 0X3F, sizeof(f));
    //假设前一列有两个空地，这里不用放障碍物，以免浪费次数
    f[0][0][0] = 0;

    // 遍历所有的列
    for (int i = 1; i <= n; i++) {
      int t1 = mp[grid[0][i - 1]], t2 = mp[grid[1][i - 1]];

      // 遍历s1和s2所有状态
      for (int s1 = 0; s1 < 4; s1++)
        for (int s2 = 0; s2 < 4; s2++) {
          // 首先尝试不放置障碍物
          update(i, s1, s2, t1, t2, 0);
          // 如果上面是空地，则尝试在上面放置一个障碍物, 其中3代表障碍物
          if (grid[0][i - 1] == '.') update(i, s1, s2, 3, t2, 1);
          // // 如果下面是空地，则尝试在下面放置一个障碍物
          if (grid[1][i - 1] == '.') update(i, s1, s2, t1, 3, 1);
          // 如果上下都是空地，则尝试上下都放置障碍物
          if (grid[0][i - 1] == '.' && grid[1][i - 1] == '.')
            update(i, s1, s2, 3, 3, 2);
        }
    }

    // 找出最小值，就是答案
    int res = INF;
    for (int i = 0; i < 4; i++)
      for (int j = 0; j < 4; j++) {
        res = min(res, f[n][i][j]);
      }
    return res;
  }

  int guardCastle(vector<string>& grid) {
    n = grid[0].size();
    m = 2;

    //把所有传送门当作恶魔
    int res = INF;
    vector<string>&& g1 = change(grid, 'S');
    res = min(res, cal(g1));

    //把所有传送门当作城堡
    vector<string>&& g2 = change(grid, 'C');
    res = min(res, cal(g2));

    return res == INF ? -1 : res;
  }
};
```



## 参考


https://leetcode.cn/problems/7rLGCR/solution/lcp-38-shou-wei-cheng-bao-by-acvving_zyy-d16y/