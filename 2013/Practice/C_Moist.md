# 2013 Practice C. Moist


## Problem/题意

Given N strings (with white spaces) standing for N cards, if current card is lexicographically smaller than the previous card, the robot select and insert it to previous cards correctly, costing $1. Compute how much money do Moist need to sort all these cards.

给定N个字符串，表示 N 个卡片，如果当前卡片词典序比前一个卡片小，那么机器人把这张卡片插入到前面的序列中，花费 1 元，求把所有的卡片排序好之后需要多少钱。


## Analysis/分析

The key is storing the largest one among sorted cards.

关键是记录已排序的卡片中最大的卡片。


## Solution/解法
Just scan it.

直接扫一遍，每次更新最大的或更新花费。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    // freopen(“2013-P-C-small-practice-1.in”, “r”, stdin);
    // freopen(“2013-P-C-small-practice-1.out”, “w”, stdout);
    int T;
    cin >> T;
    for (int t = 1; t <= T; t++) {
        int N = 0;
        cin >> N;
        vector<string> cards(N);
        string s;
        getline(cin, s);
        int ans = 0;
        for (int n = 0; n < N; n++) {
            getline(cin, cards[n]);
            // s is the largest one and the last one in the sorted ones.
            if (n == 0) {
                s = cards[n];
            }
            else {
                if (cards[n] < s) ans++;
                else s = cards[n];
            }
        }
        cout << "Case #" << t << ": " << ans << endl;
    }
    return 0;
}
```