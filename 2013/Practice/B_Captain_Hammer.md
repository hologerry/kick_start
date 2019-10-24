# 2013 Practice B. Captain Hammer


## Problem/题意
Given V (velocity) and D (distance), compute the theta.

给定抛物线的初速度和距离，求抛出的角度。

## Analysis/分析
$$
2\sin(\theta)\cos(\theta) = \frac{Dg}{V^2}
$$
$$
\theta = \frac{\arcsin(\frac{Dg}{V^2})}{2}
$$

## Solution/解法
```cpp
#include <bits/stdc++.h>
using namespace std;

int T;
const double g = 9.8;
const double Pi = acos(-1.0);

int main() {
    freopen("2013-P-B-small-practice.in", "r", stdin);
    freopen("2013-P-B-small-practice.out", "w", stdout);
    cin >> T;
    for (int t = 1; t <= T; t++) {
        double V, D;
        cin >> V >> D;
        double target = (D * g) / V / V;
        double ans = asin(target) * 0.5;
        ans = ans / Pi * 180;
        printf("Case #%d: %lf\n", t, ans);
    }
    return 0;
}
```

## Note/注意
Cant pass the OJ, even with the scoreboard solutions.

榜上的解法都过不了 OJ