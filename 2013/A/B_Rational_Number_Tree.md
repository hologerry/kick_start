# 2013 Round A B. Rational Number Tree


## Problem/题意

Rational number tree:
```
         1/1
    ______|______
    |           |
   1/2         2/1
 ___|___     ___|___
 |     |     |     |
1/3   3/2   2/3   3/1
```

root: p = 1, q = 1,
left: p = p, q = p+q, right : p = p+q, q = q;

so that the sequence of the rational numbers are:

```
1/1, 1/2, 2/1, 1/3, 3/2, 2/3, 3/1, ...
```

solve two questions:
1. Given the id (starts from 1) of the rational number, return the rational p and q;
2. Given the p and q, return the id

有理数树:
解决两个问题：
1. 给定有理数序列编号（从1开始）返回有理数的分子 p 分母  q
2. 给定有理数分子 p 分母 q, 求序列编号


## Analysis/分析
For small dataset:
1 ≤ *n*, *p*, *q* ≤ 2^16-1; *p*/*q* is an element in a tree with level number ≤ 16.
we can generate the rational number tree.

But for large dataset:
1 ≤ *n*, *p*, *q* ≤ 2^64-1; *p*/*q* is an element in a tree with level number ≤ 64.
we can’t do that.

We have to utilize the properties of tree. start from root 
```
                      1/1 1 odd
         _________________|_______________
         |                               |
     1/2 2 even                      2/1 3 odd
 ________|________             __________|__________
 |               |             |                   |
1/3 4 even   3/2 5 odd     2/3 6 even          3/1 7 odd
```
use `parent_p` stands for parent’s `p` and `parent_q` stands for parent’s `q`

we can observe this:
* for every odd node, its `p = parent_p + parent_q`  `q = parent_q`
* for every even node, its  `p = parent_p `  `q = parent_p + parent_q`

we use `n /= 2` to get the path from target node to root
through the path from root to target node, we use `n%2` to get even/odd node

For the second question:
we can observe this:
each left node, its p is smaller than its q
each right node, its p is larger than its q
we can use this iteratively find the path from the target node to root


## Solution/解法
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef unsigned long long ull;

void solve1() {
    ull n;
    cin >> n;
    vector<int> lv;
    while (n > 1) {
        lv.push_back(n%2);
        n /= 2;
    }
    reverse(lv.begin(), lv.end());
    ull p = 1, q = 1;
    for (auto v: lv) {
        if (v == 0) {
            q += p;
        }
        else {
            p += q;
        }
    }
    cout << p << " " << q << endl;
}

void solve2() {
    ull p, q;
    cin >> p >> q;
    ull now = 1, ans = 0;
    while (p != 1 || q != 1) {
        if (p > q) {
            ans += now;
            p -= q;
        }
        else {
            q -= p;
        }
        now <<= 1;
    }
    ans += now;
    cout << ans << endl;
}


int main() {
    // freopen("b.in", "r", stdin);
    freopen("2013_A_B-large-practice.in", "r", stdin);
    freopen("2013_A_B-large-practice.out", "w", stdout);

    int T;
    cin >> T;
    for (int t = 1; t <= T; t++) {
        int id;
        cin >> id;
        cout << "Case #" << t << ": ";
        if (id == 1) {
            solve1();
        }
        else if (id == 2) {
            solve2();
        }
    }
    return 0;
}
=
```

