---
title: Codeforces Round 1072 (Div. 3)
published: 2026-01-15
image: ./cover.png
tags: [Codeforces, Div.3]
category: Tutorial
draft: false
---

# ☁️体验

近一个月没怎么刷算法题了，有些生疏。整场下来难度比平时的 div.3 要高点，有点 div.2 - 2.5 的感觉。

一开始被 A 卡了 20 多分钟，没仔细考虑 n = 3 的情况，喜提 3 发 WA。

B 题纯思维，没什么好说的。

C 题很水，BFS模拟一下就好了。然而我提前跳到后面去看 G 了，导致 C 题 1:40 才过。

D 题没读明白，跳了。

E 题写了个滑动窗口，喜提 TLE。

F 题没看。

G 题线段树上二分，题目读错了两次。可惜赛时没注意到 ans 只可能是 0 或 1，TLE 了，没做出来。比赛结束之后多想了两分钟，发现这个结论之后就 AC 了。

有趣的是，这把我都做好掉大分的准备了，结果 Rating 变化是 0，居然没掉分。

# 💡题解

## A - Social Experiment

### 题意

给你一个正整数 $n$ ，表示有 $n$ 个人。

你需要把这些人分组，每组的人数必须是 $2$ 或 $3$。每组可以选择一个种类，种类只有 $2$ 种可供选择。问两个种类的总人数最少相差多少。

输出一个整数，表示答案。

### 思路

首先，当 $n$ 为 $2$ 或 $3$ 时，只能分一组，答案就是 $n$。

接下来讨论 $n > 3$ 的情况：

 - 当 $n$ 为偶数时，对于两个种类，我们总能分出两个 $\frac{n}{2}$ 人。如果 $\frac{n}{2}$ 是偶数，那就全部两两一组；否则，最后一组就放 $3$ 个人。

    *这种情况，两个种类的总人数相差 $0$。*

- 当 $n$ 为奇数时，我们可以先拿出 $5$ 个人。剩下的部分就是偶数了，按照刚刚上面的方案分组。这时两个种类，要么一样多，要么其中一种多 $2$ 人。

    - 如果一样多，就一种补 $2$ 人， 另一种补 $3$ 人；
    - 如果多 $2$ 人，就少的那种补 $2$ 人，多的那种补 $3$ 人。

    *这种情况，两个种类的总人数总是相差 $1$。*

那么 $n > 3$ 时，答案就是 $n \mod 2$。

### 代码

```cpp
void solve()
{
    int n; 
    cin >> n;

    if(n == 2 || n == 3) 
        D(n);
    else 
        D(n % 2);
}
```

## B - Hourglass

### 题意

给你 $3$ 个正整数 $s$，$k$，$m$，分别表示：
- $s$：沙漏漏完一次可以漏 $s$ 分钟
- $k$：每 $k$ 分钟翻转一次
- $m$：总时间

你需要计算出沙漏在 $m$ 分钟之后还可以漏多少分钟。

输出一个整数，表示答案。

### 思路

如何推算最终状态呢？🧐

不难算出：
- 沙漏的反转次数 
  - $n = \frac{m}{k}$
- 最后一次翻转之后，距离结束，还剩余的时间 
  - $d = m \mod k$

然后再看，最后一次翻转之后，沙漏上方还有多少分钟：
- 如果 $s > k$ 且 $n$ 为奇数，那么沙漏上方还有 $k$ 分钟；
- 容易证明，除此之外的情况，沙漏上方总是还有 $s$ 分钟。

接下来，沙漏上方还会再漏 $d$ 分钟。

我们兼容沙子不够 $d$ 分钟的情况，那么最终答案就是：

- $max(x - d, 0)$

### 代码

```cpp
void solve()
{
    int s, k, m; 
    cin >> s >> k >> m;

    int n = m / k, d = m % k;          
    int x = (s > k && (n % 2 == 1)) ? k : s;
    int ans = max(x - d, 0LL);

    D(ans);
}
```

## C - Huge Pile

### 题意

给你两个正整数 $n$ 和 $k$。

每次将 $n$ 分成两部分：

- 如果 $n$ 是偶数，那么分成两个 $\frac{n}{2}$；
- 如果 $n$ 是奇数，那么分成 $\frac{n + 1}{2}$ 和 $\frac{n - 1}{2}$。

分出的部分将会一直重复这个过程:

$1$ 个变 $2$ 个，$2$ 个变 $4$ 个，$4$ 个变 $8$ 个...

问 $n$ 最少要分多少次，才可以分出 $k$。如果不可能，则输出 $-1$。

### 思路

BFS 模拟一下就好了。☝️🤓

注意去重，防止 TLE。

### 代码

```cpp
struct step
{
    int x, s;
};

void solve()
{
    int n, k; 
    cin >> n >> k;

    if(n < k) 
    {
        D(-1);
        return;
    }

    set<int> vis;
    queue<step> q;
    q.push({n, 0});

    while(!q.empty())
    {
        auto [x, s] = q.front();

        if(x == k)
        {
            D(s);
            return;
        }

        q.pop();
        if(vis.contains(x)) 
            continue;
        vis.insert(x);

        int a = x / 2, r = x % 2;
        if(a >= k)  
            q.push({a, s + 1});
        if(r) q.push({a + 1, s + 1});
    }

    D(-1);
}
```

## D - Unfair Game

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## E - Exquisite Array

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## F - Cherry Tree 

待更新

<!-- ### 题意

### 思路

### 代码

```cpp
``` -->

## G - Nastiness of Segments 

### 题意

给你一个长度为 $n$ 的正整数数组 $a$ 以及 $q$ 次操作。

每次操作以两种形式给出：
- $1$ $p$ $x$：将第 $p$ 个元素的值改为 $x$
- $2$ $l$ $r$：询问 $a_l, a_{l + 1}, \dots, a_r$ 中有多少个元素，满足：
  - $min(a_l,a_{l+1}, \dots,a_{l+d})=d$

**注意：操作 $2$ 的范围是 $[l, r]$，而操作 $2$ 中 $min$的范围是 $[l, l + d]$。**

操作 $1$ 的影响是持久化的。

对于每次操作 $2$，你都要输出一个答案。

### 思路

很容易想到，使用线段树来解决带修改的$RMQ$。

但是，如果仅仅只用线段树，对于每次询问，我们实际上还是要枚举所有的右端点，才能确定答案。

每次查询 $O((r-l+1) \log n)$

最坏 $2 \times 10 ^ 5$ 次查询 × $2 \times 10 ^ 5$ 长度 → $4 \times 10 ^ {10}$ 次操作，必 TLE。

难道只能写离线做法卡过去了吗？😭

那太麻烦了。😣

注意到，前缀 $min$ 的值总是单调递减的，而 $d$ 的值是单调递增的。显然，每次询问区间内，只有 $1$ 个 $d$ 是合法的。

换句话说：**答案只可能是 $0$ 或 $1$**。

WTF！😱

那我们直接二分答案，看区间内的这个 $d$ 存不存在，不就行了吗？

### 代码

```cpp
void solve()
{
    int n, q;
    cin >> n >> q;

    vector<int> a(n);
    vcin(a);

    SegTree st(a);

    while (q--)
    {
        int op;
        cin >> op;
        if (op == 1)
        {
            int p, x;
            cin >> p >> x;
            st.update(p - 1, x);
        }
        else
        {
            int l, r, ans = 0;
            cin >> l >> r;
            l--, r--;
            if(l == r) 
            {
                D(0);
                continue;
            }
            int lo = 0, hi = r - l;
            while(lo <= hi) 
            {
                int mid = (lo + hi) >> 1;
                int mn = st.queryMin(l, l + mid);
                if(mn == mid) 
                {
                    ans = 1;
                    break;
                } 
                else if (mn > mid)  lo = mid + 1;
                else hi = mid - 1;
            }
            D(ans);
        }
    }
}
```

# 🎈模板部分

## 线段树 - 区间最小值查询 - 单点修改

```cpp
struct SegTree {
    int n;
    vector<int> mn;

    SegTree(const vector<int>& a) {
        n = a.size();
        mn.assign(n << 2, INF);
        build(a, 1, 0, n - 1);
    }

    void build(const vector<int>& a, int o, int l, int r) {
        if (l == r) { mn[o] = a[l]; return; }
        int mid = (l + r) >> 1;
        build(a, o << 1, l, mid);
        build(a, o << 1 | 1, mid + 1, r);
        mn[o] = min(mn[o << 1], mn[o << 1 | 1]);
    }

    void update(int pos, int v, int o, int l, int r) {
        if (l == r) { mn[o] = v; return; }
        int mid = (l + r) >> 1;
        if (pos <= mid) update(pos, v, o << 1, l, mid);
        else            update(pos, v, o << 1 | 1, mid + 1, r);
        mn[o] = min(mn[o << 1], mn[o << 1 | 1]);
    }

    int query(int L, int R, int o, int l, int r) {
        if (R < l || L > r) return INF;
        if (L <= l && r <= R) return mn[o];
        int mid = (l + r) >> 1;
        return min(query(L, R, o << 1, l, mid),
                   query(L, R, o << 1 | 1, mid + 1, r));
    }

public:
    void update(int pos, int v) { update(pos, v, 1, 0, n - 1); }
    int queryMin(int L, int R)  { return query(L, R, 1, 0, n - 1); }
};
```

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
