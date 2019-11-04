# 2013 Round B B. Meet and party


## Problem/题意

Given n rectangles defined by (x1, y1, x2, y2), there are (x2-x1+1)*(y2-y1+1) friends in one rectangle to join the party.
to join the party the cost is defined by |x1 - x2| + |y1 - y2| for two point
we have to find out the one friend's room to hold the party, and the cost is minimum

给定 n 个矩形区域，每个矩形区域里的每一个点都表示一个朋友，
每一个朋友参加聚会的花费定义为 |x1 - x2| + |y1 - y2|
找一个朋友的房间来聚会，并且使得总花费最小。


## Analysis/分析

convert n rectangles to N friend points
sort points
sort x, y respectively
calculate the prefix sum of x and y
scan the point, for each point we calculate the cost, using lower_bound find the x and y position of the x y vector, to get how many points's x and y are smaller or bigger than the point
so, with the prefix sum we can calculate the xcost and ycost, respectively

把n个区域转化为 N 个点
排序N个点
排序x坐标，y坐标
计算x坐标，y坐标的前缀和
对于每一个点，计算把这个点作为聚会点的花费，结合 lower_bound 和前缀和

```cpp
ll xcost = (ll) points[i].first * (tx + 1) - sumx[tx+1]
    + sumx.back() - sumx[tx+1] - (ll) points[i].first * (n - 1 - tx);
ll ycost = (ll) points[i].second * (ty + 1) - sumy[ty+1]
    + sumy.back() - sumy[ty+1] - (ll) points[i].second * (n - 1 - ty);
```

```
               |
            |  |
-------
      |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |

      tx
```

## Solution/解法
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main() {
    // freopen("b.in", "r", stdin);
    freopen("2013_B_B-large-practice.in", "r", stdin);
    freopen("2013_B_B-large-practice.out", "w", stdout);

    int T;
    scanf("%d", &T);
    for (int t = 1; t <= T; t++) {
        int n;
        scanf("%d", &n);
        vector<pair<int, int>> points;
        vector<int> x, y;
        vector<ll> sumx, sumy;
        for (int i = 0; i < n; i++) {
            int x1, y1, x2, y2;
            scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
            for (int tx = x1; tx <= x2; tx++) {
                for (int ty = y1; ty <= y2; ty++) {
                    points.push_back({tx, ty});
                    x.push_back(tx);
                    y.push_back(ty);
                }
            }
        }
        n = points.size();
        sort(points.begin(), points.end());
        sort(x.begin(), x.end());
        sort(y.begin(), y.end());
        sumx.push_back(0);
        sumy.push_back(0);
        for (int i = 0; i < n; i++) {
            sumx.push_back(sumx.back() + x[i]);
            sumy.push_back(sumy.back() + y[i]);
        }
        pair<int, int> ans;
        ll bestcost = 1ll << 61;
        for (int i = 0; i < n; i++) {
            int tx = lower_bound(x.begin(), x.end(), points[i].first) - x.begin();
            int ty = lower_bound(y.begin(), y.end(), points[i].second) - y.begin();
            ll xcost = (ll) points[i].first * (tx + 1) - sumx[tx+1]
                + sumx.back() - sumx[tx+1] - (ll) points[i].first * (n - 1 - tx);
            ll ycost = (ll) points[i].second * (ty + 1) - sumy[ty+1]
                + sumy.back() - sumy[ty+1] - (ll) points[i].second * (n - 1 - ty);
            ll cost = xcost + ycost;
            if (cost < bestcost) {
                bestcost = cost;
                ans = points[i];
            }
        }
        cout << "Case #" << t << ": " << ans.first << " " << ans.second << " " << bestcost << endl;
    }
    return 0;
}
```