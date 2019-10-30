# 2013 Round A C. Sorting


## Problem/题意
Given an array of integers, all odd numbers belong to one group and all even numbers belong to another group. sort all odd and even numbers, odd numbers increasing order, even numbers decreasing order.
after the sorting, odd numbers ocuppy thier origal position and so as even numbers

给定一个整数数组，将所有的奇数升序拍序，偶数降序排序，排序后奇数和偶数占用原来初试位置


## Analysis/分析
just sort


## Solution/解法

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    // freopen("c.in", "r", stdin);
    freopen("2013_A_C-large-practice.in", "r", stdin);
    freopen("2013_A_C-large-practice.out", "w", stdout);
    int T;
    cin >> T;
    for (int t = 1; t <= T; t++) {
        int n;
        cin >> n;
        vector<int> alexIdx;
        vector<int> alexVal;
        vector<int> bobIdx;
        vector<int> bobVal;
        int value;
        for (int i = 0; i < n; i++) {
            cin >> value;
            if (value % 2) {
                alexIdx.push_back(i);
                alexVal.push_back(value);
            }
            else {
                bobIdx.push_back(i);
                bobVal.push_back(value);
            }
        }
        sort(alexVal.begin(), alexVal.end());
        sort(bobVal.begin(), bobVal.end(), greater<int>());
        vector<int> ans(n);
        int i = 0;
        for (auto aidx: alexIdx) {
            ans[aidx] = alexVal[i++];
        }
        i = 0;
        for (auto bidx: bobIdx) {
            ans[bidx] = bobVal[i++];
        }
        cout << "Case #" << t << ":";
        for (auto a: ans) cout << " " << a;
        cout << endl;
    }
    return 0;
}
```