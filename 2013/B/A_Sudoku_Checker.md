# 2013 Round B A. Sudoku Checker


## Problem/题意

Given a completed sudoku matrix, check if it is valid
* Each row contains each number from 1 to N2, once each.
* Each column contains each number from 1 to N2, once each.
* Divide the N2xN2 matrix into N2 non-overlapping NxN sub-matrices. Each sub-matrix contains each number from 1 to N2, once each.

给定一个已完全填满的数独矩阵，判断是否合法


## Solution/解法

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 7;

int T, n;
int board[N*N][N*N];

unordered_set<string> st;
string t;

bool solve() {
    st.clear();
    for (int i = 0; i < n*n; i++) {
        for (int j = 0; j < n*n; j++) {
            if (board[i][j] < 1 || board[i][j] > n*n) return false;
            t = to_string(board[i][j]) + " in row " + to_string(i);
            if (st.count(t)) return false;
            st.insert(t);
            t = to_string(board[i][j]) + " in col " + to_string(j);
            if (st.count(t)) return false;
            st.insert(t);
            t = to_string(board[i][j]) + " in block " + to_string(i/n) + "-" + to_string(j/n);
            if (st.count(t)) return false;
            st.insert(t);
        }
    }
    return true;
}

int main() {
    // freopen("a.in", "r", stdin);
    freopen("2013_B_A-large-practice.in", "r", stdin);
    freopen("2013_B_A-large-practice.out", "w", stdout);

    scanf("%d", &T);
    for (int t = 1; t <= T; t++) {
        scanf("%d", &n);
        for (int i = 0; i < n*n; i++) {
            for (int j = 0; j < n*n; j++) {
                scanf("%d", &board[i][j]);
            }
        }
        bool ans = solve();
        if (ans) printf("Case #%d: Yes\n", t);
        else printf("Case #%d: No\n", t);
    }
    return 0;
}
```