---
title: 牛客周赛 Round 127
published: 2026-01-19
image: ./cover.png
tags: [牛客, 牛客周赛]
category: Tutorial
draft: false
---

# ☁️体验

比较简单的一场周赛。

# 💡题解

## A - Get The Number

### 题意

给你 $3$ 个正整数 $a$，$b$，$c$。

让你判断 $a$ 和 $b$ 能否通过加减乘除其中一种操作，得到 $c$。

其中，除法需要满足 $a$ 能被 $b$ 整除。

### 思路

直接模拟即可。

其中，$a$ 能被 $b$ 整除，换句话来说，就是 $a$ 等于 $b \times c$ 。

### 代码

```cpp
void solve()
{
    int a, b, c; 
    cin >> a >> b >> c;

    if(a + b == c || a - b == c || a * b == c || a == b * c) YES;
    else NO;
}
```

## B - Sudoku

### 题意

检验 $4 \times 4$ 版本的数独是否合法。

### 思路

有点麻烦的模拟，分类讨论即可。

### 代码

```cpp
void solve()
{
    vector<vector<int>> a(4, vector<int> (4));
    vvcin(a);
        
    vector<set<int>> c(4), r(4);

    for(int i = 0; i < 4; i++)
        for(int j = 0; j < 4; j++)
        {
            c[i].insert(a[i][j]);
            r[j].insert(a[j][i]);
        }

    for(auto& i : c)
        if(i.size() != 4)
        {
            NO;
            return;
        }
    for(auto& i : r)
        if(i.size() != 4)
        {
            NO;
            return;
        }

    vector<pair<int, int>> b = {{0, 0}, {0, 2}, {2, 0}, {2, 2}};

    for(auto& [x, y] : b)
    {
        set<int> s;
        for(int i = x; i < x + 2; i++)
            for(int j = y; j < y + 2; j++)
                s.insert(a[i][j]);
        if(s.size() != 4)
        {
            NO;
            return;
        }
    }

    YES;
}
```

## C - Carry The Bit

### 题意

给你一个正整数 $n$，你需要对 $n$ 进行以下操作恰好一次：

- 选择 $n$ 的任意一个数位，将该数位进行四舍五入。

四舍五入：

- 具体来说，设你选择的是从右往左第 k 位（$k \ge 1$，个位是第 $11$ 位），如果该位数字 $\ge 5$，则将该位及其右侧所有数位都变为 $0$，并将前一位加 $1$（必要时继续向更高位进位）；若不存在前一位（即选择的是最高位），则在数的最左端新增一个 $1$；如果该位数字 $\le 4$，则直接将该位及其右侧所有数位都变为 $0$。 

你的任务就是求出进行恰好一次操作后，能得到的最大的结果是多少。

其中：$10 \leq n < 10^{200000}$

### 思路

由于 $n$ 很大，我们使用字符串进行高精度模拟。

从前往后遍历，找到第一个大于等于 $5$ 的数位，将其进行四舍五入。

如果没有找到，说明所有的数位都小于 $5$，我们直接将最后一位进行四舍五入，使减少量最小。

### 代码

```cpp
void solve()
{
    string s; 
    cin >> s;

    int n = s.length();
    bool ok = false;

    for(int i = 0; i < n; i++)
        if(s[i] >= '5')
        {
            ok = true;
            for(int k = i; k < n; k++) 
                s[k] = '0';
            
            int j = i - 1;
            while(j >= 0 && s[j] == '9')
            {
                s[j] = '0';
                j--;
            }

            if(j >= 0) s[j]++;
            else s = "1" + s;

            break;
        }

    if(!ok) s[n - 1] = '0';

    D(s);
}
```

## D - Permutation? Counting

### 题意

给定一个长度为 $n$ 的正整数序列 $a$，问 $a$ 中有多少个子序列是双排列，将答案对 $998244353$ 取模。

双排列：

- 长度为 $2n$ 的双排列为两个长度为 $n$ 的排列打乱顺序后得到的数组。

### 思路

很明显，我们无需考虑子序列的顺序，只需要考虑子序列中元素的数量就好。

先统计每个元素的出现次数。

然后，从 $1$ 开始：

- 设 $f_i$ 为 $i$ 的出现次数，$n$ 为满足条件的最后一个数。
- 如果 $f_i \le 1$，那么到 $i$ 这个数这里，就不再满足双排列的条件，后续的数也必然不再满足，计数结束。
- 如果 $f_i \ge 2$，那么对于 $i$ 这个数，在 $f_i$ 中选择 $2$ 个元素，就有 $C_{f_i}^2$ 种选择方法。
- 因此，对于从 $1$ 到 $n$ 的双排列，就有 $\prod_{i = 1}^{n} C_{f_i}^2$ 种选择方法。
- 最后，我们的答案就是 $\sum_{i = 1}^{n} \prod_{i = 1}^{n} C_{f_i}^2$。

### 代码

```cpp
void solve()
{
    input(n, a);

    unordered_map<int, int> f;
    for(int& i : a) 
        f[i]++;

    int ans = 0, add = 1;

    for(int i = 1; f[i] >= 2; i++)
    {
        int x = f[i] * (f[i] - 1) / 2 % mod;
        add = add * x % mod;
        ans = (ans % mod + add % mod) % mod;
    }

    D(ans % mod);
}
```

## E - Balanced 01-String

### 题意

给你一个 $01$ 字符串 $s$。

但是其中有些位是 '$?$'，你可以任意填入 '$0$' 或 '$1$'。

如果相邻相同的对数是偶数，那么这个字符串就是平衡的。

问有多少种不同的填入方法，使得该字符串平衡，并将答案对 $998244353$ 取模。

### 思路

平衡的01串，其相同对数的个数是偶数。

设长度为 $n$，可以得出：

$相同对数 + 不同对数 = n - 1$

我们可以从不同对数的角度来突破这个问题。

那么，平衡的01串，满足其中一个条件即可：

- 不同对数的个数是偶数，并且 $n - 1$ 是偶数；
- 不同对数的个数是奇数，并且 $n - 1$ 是奇数。

现在设相邻不同对数为 $x$。

那一对相邻对，能对 $x$ 产生的贡献是多少？

- 如果 $s_i \neq s_{i+1}$，$x$ 就会加 $1$。
- 如果 $s_i = s_{i+1}$，就没有贡献。

不难发现，这简直和异或一模一样。

那么，按照题目给出的定义，我们可以得出：

$x = \sum_{i=1}^{n-1} s_i \oplus s_{i+1}$

$x \mod 2 = \oplus_{i=1}^{n-1} s_i \oplus s_{i+1} = s_1 \oplus s_n$ 

显然，中间的 '$?$' 可以随便填。

根据上面的分析，我们可以得出：

两端的异或值需要与 $n$ 的奇偶性相同。

设 '$?$' 的个数为 $q$：

- 如果两端都不是 '$?$'，并且已经满足奇偶关系，那答案就是 $2^q$；
- 如果两端都不是 '$?$'，但是不满足奇偶关系，那答案就是 $0$；
- 否则，答案为 $2^{q-1}$。

### 代码

```cpp
vector<int> p(500001);

void solve()
{
    string s; 
    cin >> s;

    int n = s.length(), ans = 0;

    if(n == 1) 
    {
        cout << (s[0] == '?' ? 2 : 1) << endl;
        return;
    }

    int q = 0;
    for(char& c : s) 
        q += c == '?';

    char l = s[0], r = s[n - 1];
    if(l != '?' && r != '?')
    {
        int a = l - '0', b = r - '0';
        if(a ^ b == (n - 1) % 2) 
            ans = p[q];
    }

    D(ans % mod);
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    p[0] = 1;
    for(int i = 1; i <= 500000; i++)
        p[i] = p[i - 1] * 2 % mod;

    int _ = 1;
cin >> _;
    while (_--)
        solve();
    return 0;
}
```

## F - Cherry Tree 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

# 🎈模板部分

## 火车头

```cpp
#include <bits/stdc++.h>
#define inf INT_MAX
#define INF LLONG_MAX
#define MOD 1000000007
#define mod 998244353
#define all(_x) _x.begin(), _x.end()
#define vcin(_x) for(auto& _i : _x) cin >> _i
#define vvcin(_x) for(auto& _j : _x) for(auto& _i : _j) cin >> _i
#define D(_x) cout << _x << endl
#define vD(_x) for(int _i = 0; _i < _x.size(); ++_i) cout << _x[_i] << " \n"[_i == _x.size() - 1]
#define input(_n, _a) int _n; cin >> _n; vector<int> _a(_n); vcin(_a)
#define flr(_i, _l, _r) for(int _i = _l; _i <= _r; ++_i)
#define frl(_i, _r, _l) for(int _i = _r; _i >= _l; --_i)
#define YES cout << "YES" << endl
#define NO cout << "NO" << endl
#define Yes cout << "Yes" << endl
#define No cout << "No" << endl
#define yes cout << "yes" << endl
#define no cout << "no" << endl
using namespace std;

#define int long long
#define endl '\n' 

void solve()
{
    
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int _ = 1;
cin >> _;
    while (_--)
        solve();
    return 0;
}
```
