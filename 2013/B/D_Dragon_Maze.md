# 2013 Round B D. Dragon Maze


## Problem/题意
Given a maze with start and exit cell, for each cell you cannot go (-1) or gain the power(>0), You have to get out of the maze with shortest path and largest power

给定一个迷宫包含了起始和退出点，对于每一个格子，要么不能去，要么得到格子对应的能量，求从起始点到结束点最短路且最大的能量收益


## Analysis/分析
We utilize the BFS to find the shortest path, and use the DP to get largest one

用BFS求最短路，用 DP 求最大收益


## Solution/解法
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 105;

vector<int> dx = {1, -1, 0, 0};
vector<int> dy = {0, 0, 1, -1};
// first: level, second: cur powere
pair<int, int> dist[N][N];
int maze[N][N];

int main() {
    // freopen("d.in", "r", stdin);
    freopen("2013_B_D-small-practice.in", "r", stdin);
    freopen("2013_B_D-small-practice.out", "w", stdout);
    int T;
    scanf("%d", &T);
    for (int t = 1; t <= T; t++) {
        int n, m;
        scanf("%d%d", &n, &m);
        int sx, sy, gx, gy;
        scanf("%d%d%d%d", &sx, &sy, &gx, &gy);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                scanf("%d", &maze[i][j]);
                dist[i][j] = {INT_MAX, INT_MIN};
            }
        }
        queue<pair<int, int>> q;
        q.push({sx, sy});
        dist[sx][sy] = {0, maze[sx][sy]};
        while (q.size()) {
            auto cur = q.front(); q.pop();
            int x = cur.first, y = cur.second;
            for (int i = 0; i < 4; i++) {
                int nx = x + dx[i], ny = y + dy[i];
                if (0 <= nx && nx < n && 0 <= ny && ny < m && maze[nx][ny] != -1) {
                    if (dist[nx][ny].first < dist[x][y].first + 1) {
                        // visited
                        continue;
                    }
                    if (dist[nx][ny].first == INT_MAX) {
                        // never visited before
                        q.push({nx, ny});
                    }
                    dist[nx][ny].first = dist[x][y].first + 1;
                    // DP
                    // 同一层只要最大的那个
                    dist[nx][ny].second = max(dist[nx][ny].second, dist[x][y].second + maze[nx][ny]);
                }
            }
        }
        if (dist[gx][gy].first == INT_MAX) {
            printf("Case #%d: Mission Impossible.\n", t);
        }
        else {
            printf("Case #%d: %d\n", t, dist[gx][gy].second);
        }
    }
    return 0;
}
```