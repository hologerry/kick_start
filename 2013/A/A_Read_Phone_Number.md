# 2013 Round A  A. Read Phone Number


## Problem/题意
Give a string stands for phone number, and given a format stands consider how many numbers together. read the phone number.

Single numbers just read them separately.
2 successive numbers use double.
3 successive numbers use triple.
4 successive numbers use quadruple.
5 successive numbers use quintuple.
6 successive numbers use sextuple.
7 successive numbers use septuple.
8 successive numbers use octuple.
9 successive numbers use nonuple.
10 successive numbers use decuple.
More than 10 successive numbers read them all separately.

Example:
15012233444 3-4-4
one five zero one double two three three triple four

给定一串数字，表示电话号码，另外给定一个读的格式，将这个电话号码读出来


## Analysis/分析
split the format string
parse the phone number with the format number one by one

将格式字符串分开，然后按一个段长度一个一个处理


## Solution/解法
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<string> strs = {“”, “”, “double”, “triple”, “quadruple”, “quintuple”, “sextuple”, “septuple”, “octuple”, “nonuple”, “decuple”};
vector<string> names = {“zero”, “one”, “two”, “three”, “four”, “five”, “six”, “seven”, “eight”, “nine”, “ten”};

vector<int> split(string format, char del) {
    vector<int> res;
    stringstream s(format);
    string item;
    while (getline(s, item, del)) {
        res.push_back(stoi(item));
    }
    return res;
}

vector<string> parse(string phone, vector<int>& fors) {
    vector<string> res;
    int n = phone.size();
    int ptr = 0;
    for (auto f: fors) {
        int i = 0;
        while (i < f && ptr < n) {
            int cnt = 0;
            char curc = phone[ptr];
            while (i < f && ptr < n && curc == phone[ptr]) {
                i++;
                ptr++;
                cnt++;
            }
            if (cnt == 1) res.push_back(names[curc-‘0’]);
            else if (1 < cnt && cnt <= 10) {
                res.push_back(strs[cnt]);
                res.push_back(names[curc-‘0’]);
            }
            else if (cnt > 10) {
                while (cnt—) {
                    res.push_back(names[curc-'0']);
                }
            }
        }
    }
    return res;
}

int main() {
    // freopen("a.in", "r", stdin);
    freopen("2013_A_A-large-practice.in", "r", stdin);
    freopen("2013_A_A-large-practice.out", "w", stdout);

    int T;
    cin >> T;
    for (int t = 1; t <= T; t++) {
        string phone, format;
        cin >> phone >> format;
        vector<int> fors = split(format, '-');
        vector<string> ans = parse(phone, fors);
        string anstr = "";
        for (int i = 0; i < ans.size(); i++) {
            anstr += ans[i];
            if (i < ans.size()-1) anstr += " ";
        }
        cout << "Case #" << t << ": " << anstr << endl;
    }
    return 0;
}
```