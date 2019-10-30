# 2013 Round A E. Spaceship Defence


## Problem/题意

Given many rooms with color, if rooms with same color, we can transport within these rooms instantly. Given m turbolift which can take you from room a to room b in time t;
given room pairs p and q, find the minimum time to travel from p to q, if it's impossible, return -1;

给定很多的房间，每个房间有一种颜色，所有颜色相同的房间可以进行相互瞬移，给定 m 个运输器，可以把你从房间 a 运送到房间 b，花费 t
给定很多房间对 p 和 q，求最少的花费，如果不可行，则返回 -1


## Analysis/分析
convert room id to color id
multi-source shortest path problem
=> Floyd
note:
we only store the shortest cost at initialization
calculate once for each test case
c is way faster than c++ stl

将房间号转化为颜色号
要求多次最短路，所以是多源最短路问题
Floyd 算法
注意：
在初始化记录花费时，只记录最短的
每一个测试用例，只进行一次计算
c++ stl 比 c 慢非常非常多


## Solution/解法
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 80002, C = 2048;

int colorNum;
char color[4], colors[C][4];
int room2colorid[N];
int dist[C][C];

bool equal(char* s, char* t) {
    while (*s == *t && *s) {
        s++;
        t++;
    }
    return *s == *t;
}

int f(char* s) {
    for (int i = 0; i < colorNum; i++) {
        if (equal(s, colors[i]))
            return i;
    }
    for (int i = 0; s[i]; i++) {
        colors[colorNum][i] = s[i];
        colors[colorNum][i+1] = 0;
    }
    colorNum++;
    return colorNum - 1;
}

int main() {
    // ios_base::sync_with_stdio(false);
    // cin.tie(NULL);
    // freopen("e.in", "r", stdin);
    freopen("2013_A_E-large-practice.in", "r", stdin);
    freopen("2013_A_E-large-practice.out", "w", stdout);

    int T;
    scanf("%d", &T);
    for (int t = 1; t <= T; t++) {
        int n, m, s;
        scanf("%d", &n);

        colorNum = 0;

        for (int i = 0; i < n; i++) {
            scanf("%s", color);
            room2colorid[i] = f(color);
        }
        for (int i = 0; i < colorNum; i++) {
            for (int j = 0; j < colorNum; j++) {
                dist[i][j] = 1 << 29;
            }
            dist[i][i] = 0;
        }
        scanf("%d", &m);
        for (int i = 1; i <= m; i++) {
            int a, b, t;
            scanf("%d%d%d", &a, &b, &t);
            a--; b--;
            if (dist[room2colorid[a]][room2colorid[b]] > t)
                dist[room2colorid[a]][room2colorid[b]] = t;
        }

        for (int k = 0; k < colorNum; k++) {
            for (int i = 0; i < colorNum; i++) {
                for (int j = 0; j < colorNum; j++) {
                    if (dist[i][j] > dist[i][k] + dist[k][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }
        for (int i = 0; i < colorNum; i++) {
            for (int j = 0; j < colorNum; j++) {
                if (dist[i][j] > (1 << 28)) {
                    dist[i][j] = -1;
                }
            }
        }
        scanf("%d", &s);
        printf("Case #%d:\n", t);

        for (int i = 1; i <= s; i++) {
            int p, q;
            scanf("%d%d", &p, &q);
            p--; q--;
            int pcolorid = room2colorid[p];
            int qcolorid = room2colorid[q];
            printf("%d\n", dist[pcolorid][qcolorid]);
        }
    }
    return 0;
}
```