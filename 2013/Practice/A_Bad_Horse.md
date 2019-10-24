# 2013 Practice A. Bad Horse


## Problem/题意
For each test case, given *M* name pairs, each pair stands for two horse with troublesome, we have split all these pairs (2M horses) into two groups. Check whether we can split.

对于每一个测试例，给定M对名字对，每一对名字表示这两头马有冲突，我们要把这 2M 匹马，放到两个组里，以便每一个组里没有两头马有冲突。


## Analysis/分析
Check each connected component, judge if it is a bipartite graph.

判断一个图的每一个连通分支是否是二部图


## Solution/解法
from Algorithm Design textbook:
we perform BFS, coloring s red, all of layer L1 blue, all of layer L2 red, and so on, coloring odd-numbered layers blue and even-numbered layers red.
We can implement this on top of BFS, by simply taking the implementation of BFS and adding an extra array Color over the nodes. Whenever we get to a step in BFS where we are adding a node v to a list L[i + 1], we assign Color[v] = red if i + 1 is an even number, and Color[v] = blue if i + 1 is an odd number. At the end of this procedure, we simply scan all the edges and determine whether there is any edge for which both ends received the same color. Thus, the total running time for the coloring algorithm is O(m + n), just as it is for BFS.

利用 BFS，把 s 着红色，L1 层的节点 蓝色，L2 层的节点 红色，如此继续下去。
着色完之后判断每一条边的两个节点是否着了不同的颜色。时间复杂度 O(m+n)

## CPP
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<string, int> nameToID;
unordered_map<int, int> idToColor;

int M;
int T;

int main() {
    freopen(“2013-P-A-small-practice-2.in”, “r”, stdin);
    freopen(“2013-p-A-small-practice-2.out”, “w”, stdout);
    cin >> T;
    for (int t = 1; t <= T; t++) {
        nameToID.clear();
        idToColor.clear();
        cin >> M;
        vector<pair<int, int>> edges(M);
        int id = 0;
        for (int m = 0; m < M; m++) {
            string n1, n2;
            cin >> n1 >> n2;
            if (nameToID.find(n1) == nameToID.end()) {
                nameToID[n1] = id++;
            }
            if (nameToID.find(n2) == nameToID.end()) {
                nameToID[n2] = id++;
            }
            int id1 = nameToID[n1], id2 = nameToID[n2];
            edges[m] = {id1, id2};
        }
        unordered_map<int, vector<int>> graph;
        for (auto e: edges) {
            graph[e.first].push_back(e.second);
        }
        queue<int> que;
        vector<bool> vis(id, false);
        for (int i = 0; i < id; i++) {
            if (vis[i]) continue;
            que.push(i);
            int level = 0;
            while (que.size()) {
                int sz = que.size();
                for (int i = 0; i < sz; i++) {
                    int cur = que.front(); que.pop();
                    vis[cur] = true;
                    if (idToColor.find(cur) == idToColor.end()) {
                        idToColor[cur] = level % 2;
                    }
                    for (auto to: graph[cur]) {
                        if (vis[to] == false)
                            que.push(to);
                    }
                }
                level++;
            }
        }

        cout << “Case #” << t << “: “;
        bool can’t = false;
        for (auto e: edges) {
            if (idToColor[e.first] == idToColor[e.second]) {
                can’t = true;
                break;
            }
        }
        if (cant) cout << "No" << endl;
        else cout << "Yes" << endl;
    }
    return 0;
}
```

