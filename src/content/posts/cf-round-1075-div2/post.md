---
title: Codeforces Round 1075 (Div. 2)
published: 2026-01-24
image: ./cover.png
tags: [Codeforces, Div.2]
category: Tutorial
draft: false
---

# ☁️体验

不枉我苦练位运算，C 直接秒了。

:spoiler[其实也没练多少]

# 💡题解

## A - Table with Numbers 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## B - The Curse of the Frog 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## C1 - XOR Convenience (Easy Version) 

### 题意

给你一个正整数 $n$，你需要构造一个长度为 $n$ 的排列 $p$。

位于位置 $i$ 的数 $p_i$ 必须满足条件：

$p_i \oplus i$ 位于 $p_i$ 的右边。

其中，$2 \leq i \leq n - 1$。

输出任意一个满足条件的排列 $p$。

### 思路

注意到，从 $3$ 和 $2$ 开始：

$(3 + k) \oplus (2 + k) = 1$

那位置 $2, 3, 4, 5, \dots$

就可以填 $3, 2, 5, 4, \dots$

根据异或的性质：$a \oplus b = c \iff a \oplus c = b$

这些位置都需要右边有 $1$。

所以，我们可以这样构造排列：

如果 $n$ 是奇数，第一个位置就放 $n - 1$，否则放 $n$。

然后，放 $n - 2$ 个数，按照 $3, 2, 5, 4, \dots, 3+k, 2+k$ 的顺序放，直到放满 $n - 2$ 个数。

最后，放 $1$。

---

赛后看了其他人的代码，发现中间部分其实可以直接合并成一种情况：

```cpp
for(int i = 2; i < n; i++) 
    cout << (i ^ 1) << ' ';
```

---

### 代码

```cpp
void solve()
{
    int n; 
    cin >> n;

    cout << (n % 2 ? n - 1 : n) << ' ';

    int i = 2, l = 3, r = 2;
    while(i < n)
    {
        cout << l << ' ';
        l += 2;
        i++;

        if(i >= n) break;

        cout << r << ' ';
        r += 2;
        i++;
    }

    cout << 1 << ' ';

    cout << endl;
}
```

## C2 - XOR Convenience (Hard Version) 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## D1 - Little String (Easy Version) 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## D2 - Little String (Hard Version) 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## E - Majority Wins? 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp

``` -->

## F - Zhora the Vacuum Cleaner 

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
