## [题目链接](https://codeforces.com/contest/2111)

## [A. Energy Crystals](https://codeforces.com/contest/2111/problem/A)

### 题意

给三个数，初始值为 $0$ ，每次操作可以将其中一个数加上任意正数，在执行操作前必须满足任意一个数都小于等于任意其他数的一半（向下取整），求将三个数均变为 $x$ 的最少操作数。

### 思路

可以发现，第一步肯定是将两个 $0$ 变为 $1$ ，剩下一个 $0$ 可变为 $2/3$ ，可以发现在相同的 $2$ 的幂次区间内变为 $x$ 需要的操作数是相同的，通过枚举则可得出规律 $res = ({\lfloor} log_2x{\rfloor} + 1) * 2 + 1$

### 示例代码

```cpp
void solve() {
    int x;
    cin >> x;
    int a = __lg(x);
    cout << (a + 1) * 2 + 1 << '\n';
}
```

## [B. Fibonacci Cubes](https://codeforces.com/contest/2111/problem/B)

### 题意

有 $n$ 个立方体，其中第 $i$ 个边长为 $fi$ ，$f$ 是斐波拉契数列的值。$m$ 次询问，问一个 $w×l×h$ 的盒子能不能放下所有立方体。每个立方体不能有地方悬空，要么放在地面上要么放在另一个立方体上。

### 思路

先考虑最大的方块， $min(w, l, h) >= f[n]$，并且得满足可以将第 $n - 1$ 块能够放在他的上面，即 $max(w, l, h) >= f[n] + f[n - 1]$。其他方块可以通过迭代以相同的方式放置

### 算法思想

贪心

### 示例代码

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    fer(i, 0, m) {
        int mn = inf, mx = -inf;
        int a, b, c;
        cin >> a >> b >> c;
        mn = min({mn, a, b, c});
        mx = max({mx, a, b, c});

        if(mx < f[n] + f[n - 1] || mn < f[n]) {
            cout << '0';
        } else {
            cout << '1';
        }
    }
    cout << '\n';
}
```

## [C. Equal Values](https://codeforces.com/contest/2111/problem/C)

### 题意

对一个数组，一次操作是选一个下标 $i$ ，将其所有左边的数变为 $a_i$ ，代价为 $(i - 1) * a_i$ ，并且将其所有右边的数变为 $a_i$ ，代价为 $(n - i) * a_i$ 。求将数组变为相同值的最小代价。

### 思路

双指针，枚举每个相同数的区间，计算区间对应的代价，线性时间复杂度。

### 算法思想

快慢指针

### 示例代码

```cpp
void solve() {
    int n;
    cin >> n;
    ll res = LLONG_MAX;
    vector<int> arr(n + 1);

    fer(i, 1, n + 1) cin >> arr[i];

    int l = 1, r = 1;
    fer(i, 2, n + 1) {
        if(arr[i] != arr[i - 1]) {
            ll s = 1LL * (l - 1) * arr[l] + 1LL * (n - r) * arr[r];
            res = min(res, s);
            l = i, r = i;
        } else {
            r++;
        }
    }
    res = min(res, 1LL * (l - 1) * arr[l] + 1LL * (n - r) * arr[r]);
    cout << res << '\n';
}
```

## [D. Creating a Schedule](https://codeforces.com/contest/2111/problem/D)

### 题意

有$n$ 个小组，每组 $6$ 节课，有 $m$ 间教室，教室都在不同楼层上，需要安排每个小组每节课的教室位置，使得所需要爬的楼层数总和最大。

### 思路

贪心，将楼层排序后，取前 $n/2$ 和后 $n/2$ 处的教室，然后需要考虑 $n$ 为奇数的情况，将落单的组跟随前 $n/2$ 或后 $n/2$ 组。

### 算法思想

贪心

### 示例代码

```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector<int> arr(m);

    fer(i, 0, m) cin >> arr[i];
    sort(all(arr));

    int res = 0;
    fer(i, 0, n / 2) {
        fer(j, 0, 6) {
            if(j & 1) {
                cout << arr[i] << ' ';
            } else {
                cout << arr[m - 1 - i] << ' ';
            }
        }
        cout << '\n';
    }

    fer(i, 0, n / 2) {
        fer(j, 0, 6) {
            if(j & 1) {
                cout << arr[m - 1 - i] << ' ';
            } else {
                cout << arr[i] << ' ';
            }
        }
        cout << '\n';
    }
    if(n & 1) {
        fer(i, 0, 6) {
            if(i & 1) cout << arr[n / 2] << ' ';
            else cout << arr[m - 1 - n / 2] << ' ';
        }
        cout << '\n';
    }
}
```

## [E. Changing the String](https://codeforces.com/contest/2111/problem/E)

### 题意

给你一个只包含 $a,b,c$ 的字符串，有 $m$ 次操作，每次让你把一类字符的一个变成另一个字符。你也可以无视任意这个操作。求最小字典序。

### 思路

用 `set` 记录每次操作，注意一定要记录操作的顺序，然后考虑什么时候需要替换。
- 对于 $a$ 无需进行操作
- 对 $b$ ，优先考虑 $b => a$ ，否则考虑 $b => c => a $
- 对 $c$ ，优先考虑 $c => a$ ，其次考虑 $c => b => a$ ，否则考虑 $c => b$

> 为什么采用 `set` 而不是`vector`呢，因为后续的`erase`和查找操作时间复杂度为 $O(logN)$ ，效率相比之下高许多。

### 示例代码

```cpp
void solve() {
    int n, q;
    cin >> n >> q;
    string s;
    cin >> s;
    set<int> st[3][3];

    fer(i, 0, q) {
        char x, y;
        cin >> x >> y;
        st[x - 'a'][y - 'a'].insert(i);
    }
    fer(i, 0, n) {
        if(s[i] == 'a') continue;

        if(s[i] == 'b') {
            if(st[1][0].size()) {
                st[1][0].erase(st[1][0].begin());
                s[i] = 'a';
            } else if(st[1][2].size()) {
                if(st[2][0].size()) {
                    auto it = st[2][0].upper_bound(*st[1][2].begin());
                    if(it != st[2][0].end()) {
                        st[1][2].erase(st[1][2].begin());
                        st[2][0].erase(it);
                        s[i] = 'a';
                    }
                }
            }
        } else if(s[i] == 'c') {
            if(st[2][0].size()) {
                st[2][0].erase(st[2][0].begin());
                s[i] = 'a';
            } else if(st[2][1].size()) {
                if(st[1][0].size()) {
                    auto it = st[1][0].upper_bound(*st[2][1].begin());
                    if(it != st[1][0].end()) {
                        st[2][1].erase(st[2][1].begin());
                        st[1][0].erase(it);
                        s[i] = 'a';
                    } else {
                        st[2][1].erase(st[2][1].begin());
                        s[i] = 'b';
                    }
                } else {
                    st[2][1].erase(st[2][1].begin());
                    s[i] = 'b';
                }
            }
        }
    }
    cout << s << '\n';
}
```



## F. Wildflower

### 题意

给一棵树，根节点编号为1，每个节点的值为 $1$ 或 $2$ ，所有值按照节点编号构成数组。现在要求所有子树包含的节点权值之和两两不同，求有多少重方法给节点赋值。

### 思路

分为以下几种情况：
1. 如果有多余两个叶子节点，那么方案数一定为 $0$，因为叶子节点权值一定会有重复的情况;
2. 如果叶子节点只有一个，那么所有节点权值均有两种情况，方案数为 $2^n$ ;
3. 如果有两个叶子节点，那么需要考虑两个方面：深度是否相同、$1$ 放在深度较大的和放在深度较小的叶子节点上产生的方案数是否相同
	- 记叶子节点为 $u, v$ ，最近公共祖先为 $p$，根节点深度为 $0$
	- 如果深度相同：那么根据题意画一画，就能发现方案数是 $2^{depth[p] + 1}$
	- 如果深度不同
		- 较大的叶子节点给 $1$: 方案数为 $2^{depth[u] - depth[v] + depth[p]}$
		- 较大的叶子节点给 2: 方案书为 $2^{depth[u] - depth[v] + depth[p] + 1}$
		- 两者加起来，总方案数为 $3 * 2^{depth[u] - depth[v] + depth[p]}$
4. 注意题目要求取模
5. 取模前应用 $long long$ 防止溢出
6. 可以学习使用自动取模类，模板可以自行搜索

### 算法思想

LCA(最近公共祖先)算法，可以解决很多树上问题

### 示例代码
```cpp
void solve() {
    int n;
    cin >> n;
    vector<vector<int>> adjlist(n + 1);
    int max_k = 18;
    vector<int> depth(n + 1);
    vector<vector<int>> fa(max_k + 1, vector<int>(n + 1));

    fer(i, 0, n - 1) {
    	int u, v;
    	cin >> u >> v;
    	adjlist[u].push_back(v);
    	adjlist[v].push_back(u);
    }

    function<void(int, int)> dfs = [&](int u, int p) {
    	fa[0][u] = p;
    	depth[u] = p ? depth[p] + 1 : 0;

    	for(int v : adjlist[u]) {
    		if(v != p) {
    			dfs(v, u);
    		}
    	}
    };

    auto init_lca = [&]() -> void {
    	dfs(1, 0);

    	fer(k, 1, max_k) {
    		fer(i, 1, n + 1) {
    			int mid = fa[k - 1][i];
    			fa[k][i] = mid ? fa[k - 1][mid] : 0;
    		}
    	}
    };

    auto lca = [&](int u, int v) -> int {
    	if(depth[u] < depth[v]) swap(u, v);
    	int diff = depth[u] - depth[v];

    	fer(k, 0, max_k) {
    		if(diff & (1 << k)) {
    			u = fa[k][u];
    		}
    	}
    	if(u == v) return u;

    	ferd(k, max_k, 0) {
    		if(fa[k][u] && fa[k][u] != fa[k][v]) {
    			u = fa[k][u];
    			v = fa[k][v];
    		}
    	}
    	return fa[0][u];
    };

    auto fpow = [&](ll a, int b)  {
    	ll res = 1 % MOD;
    	while(b) {
    		if(b & 1) res = 1LL * res * a % MOD;
    		a = a * a % MOD;
    		b >>= 1;
    	}
    	return res;
    };

    vector<int> leaf;
    fer(i, 2, n + 1) {
    	if(adjlist[i].size() == 1) leaf.push_back(i);
    }

    if(leaf.size() > 2) {
    	cout << 0 << '\n';
    	return;
    } else if(leaf.size() == 1) {
    	cout << fpow(2, n) << '\n';
    	return;
    }

    init_lca();
    int u = leaf[0], v = leaf[1];
    int ans = 0, d = depth[lca(u, v)];
    if(depth[u] == depth[v]) {
    	ans = fpow(2, d + 2);
    } else {
    	ans = 3 * fpow(2, abs(depth[u] - depth[v]) + d) % MOD;
    }
    cout << ans << '\n';
}
```