# 2013 Round A D. Cross the maze


## Problem/问题

Given a maze ('#' is wall) with start point (sx, sy) and exit point (ex, ey) (all coordinates start from 1), Judge if Edison can get out of the maze within 10000 steps, if so print out the number of steps and the direction of all steps. At each step, he puts his hand on his left wall, and keep going.

给定一个迷宫('#'表示墙壁)，开始位置和出口分别为 (sx, sy) 和 (ex, ey) , 判断 Edison 是否能在 10000 步以内走出迷宫，如果能 输出他需要走多少步，以及每一步的方向，在每一步中 Edison 总是把左手放到墙上，摸着走。

## Analysis/分析

We should determine the initial direction and, at each step we should keep trying the left direction, if left is not available we should keep current direction, if the next position with current direction is not available, we should rotate back to get correct direction with available position for next step.

我们首先应该判断初始方向，在每一步中，我们应该一直尝试左边的位置，如果左边不能用，则尝试保持当前方向，走下一个位置，如果当前方向下一个位置不能走，则往一直向右转，直到某一个方向的下一个位置可以走。


## Solution/解法

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;

vector<int> dx = {0, -1, 0, 1};
vector<int> dy = {1, 0, -1, 0};
string ds = "ENWS";

bool checkleft(int x, int y, int d, vector<string>& maze) {
    d = (d + 1) % 4;
    x += dx[d];
    y += dy[d];
    return (0 <= x && x < n && 0 <= y && y < n && maze[x][y] == '.');
}

bool checkst(int x, int y, int d, vector<string>& maze) {
    x += dx[d];
    y += dy[d];
    return (0 <= x && x < n && 0 <= y && y < n && maze[x][y] == '.');
}

int main() {
    // freopen("d.in", "r", stdin);
    freopen("2013_A_D-large-practice.in", "r", stdin);
    freopen("2013_A_D-large-practice.out", "w", stdout);
    int T;
    cin >> T;
    for (int t = 1; t <= T; t++) {
        cin >> n;
        vector<string> maze(n);
        for (int i = 0; i < n; i++) {
            cin >> maze[i];
        }
        int sx, sy, ex, ey;
        cin >> sx >> sy >> ex >> ey;
        sx -= 1, sy -= 1, ex -= 1, ey -= 1;
        int d = 0;
        cout << "Case #" << t << ": ";
        if (sx == 0 && sy == 0) {
            d = 0;
        } else if (sx == 0 && sy == n-1) {
            d = 3;
        } else if (sx == n-1 && sy == n-1) {
            d = 2;
        } else {
            d = 1;
        }
        if (sx == ex && sy == ey) {
            cout << 0 << endl;
            cout << endl;
        }
        string ans;
        bool ok = true;
        int i;
        for (i = 1; i <= 10000; i++) {
            if (checkleft(sx, sy, d, maze)) {
                d = (d+1) % 4;
                sx += dx[d];
                sy += dy[d];
                ans += ds[d];
            }
            else {
                for (int j = 0; j < 4; j++) {
                    if (checkst(sx, sy, d, maze)) break;
                    d = (d+3) % 4;  // 往回转
                }
                if (checkst(sx, sy, d, maze) == false) {
                    ok = false;
                    break;
                }
                sx += dx[d];
                sy += dy[d];
                ans += ds[d];
            }
            if (sx == ex && sy == ey) {
                cout << i << endl;
                cout << ans << endl;
                break;
            }
        }
        if (!ok || i > 10000)
            cout << "Edison ran out of energy." << endl;
    }
    return 0;
}
```