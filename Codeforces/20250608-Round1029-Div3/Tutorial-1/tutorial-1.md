[题目链接](https://codeforces.com/contest/2117)

## A. False Alarm

### 题意

需要通过 $n$ 扇门，给出一定数量已经开的门（一定存在没开的门），有一次机会使得所有门都开通 $x$ 分钟，问能否通过所有门。

### 思路

求出第一扇和最后一扇关着的门的间距与 $x$ 相比即可。

### 示例代码

```cpp
void solve() {
    int n, x;
    cin >> n >> x;
    vector<int> arr(n);

    fer(i, 0, n) cin >> arr[i];
    int a = -1, b = -1;

    fer(i, 0, n) {
        if(arr[i] == 1) {
            a = i;
            break;
        }
    }
    ferd(i, n - 1, 0) {
        if(arr[i] == 1) {
            b = i;
            break;
        }
    }
    if(a == -1 || b == -1) {
        cout << "YES" << '\n';
    }
    cout << (b - a + 1 <= x ? "YES" : "NO") << '\n';
}
```

## B. Shrink

### 题意

构造一个长度为 $n$ 的数组，每次的操作为：如果连续三个元素中中间的元素为最大值，则可将其删除。输出构造的数组，使得可操作次数最大。

### 思路

贪心
1. 思路1：将 $[1, n]$ 首尾首尾依次摆放
2. 将 $1$ 放最前面，然后剩余的数逆序摆放

### 示例代码

```cpp
void solve() {
    int n;
    cin >> n;

    vector<int> arr(n);
    int cnt = 1;
    int i = 0, j = n - 1;
    while(i <= j) {
        if(cnt & 1) {
            arr[i++] = cnt++;
        } else {
            arr[j--] = cnt++;
        }
    }
    for(int x : arr) {
        cout << x << ' ';
    }
    cout << '\n';
}
```
```cpp
void solve() {
    int n;
    cin >> n;
    cout << 1 << ' ';
    ferd(i, n, 2) cout << i << ' ';
    cout << '\n';
}
```


## C. Cool Partition

### 题意

对数组进行分区，所有元素都在一个区间内，并且相邻的两个区间前区间为后区间的子集，求划分后总区间个数的最大值。

### 思路

将第一个数为单独一个区间，用 `set` 存每个区间，遇到可覆盖情况重置两个 `set` 即可

### 示例代码

```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> arr(n);
    fer(i, 0, n) cin >> arr[i];

    int ans = 1;
    set<int> st1, st2;
    st1.insert(arr.front());

    int cnt = 0;
    fer(i, 1, n) {
        if(st1.count(arr[i]) && !st2.count(arr[i])) {
            cnt++;
        }
        st2.insert(arr[i]);
        if(cnt == st1.size()) {
            ans++, cnt = 0;
            st1 = st2;
            st2.clear();
        }
    }
    cout << ans << '\n';
}
```

## D. Retaliation

### 题意

给定一个数组，对每个索引 $i$ ，可以对所有 $a_i$ 同时进行减 $i$ 或者减 $n - i + 1$ 的操作，问能否最终数组所有元素变为 $0$ 。

### 思路

高斯消元，数组每个元素都是由 $i, n - i + 1$ 线性组合的，因此算前后相邻两组的 $x, y$ 是否都相等即可。注意处理特殊情况。
### 示例代码

```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> arr(n + 1);
    vector<pii> p(n + 1);

    fer(i, 1, n + 1) {
    	cin >> arr[i];
    	p[i].first = i, p[i].second = n - i + 1;
    }

    int yy = -1, xx = -1;
    fer(i, 2, n + 1) {
    	int a = p[i - 1].first, b = p[i - 1].second;
    	int c = p[i].first, d = p[i].second;

    	if(b * c == a * d) continue;

    	if((c * arr[i - 1] - a * arr[i]) % (b * c - a * d) != 0) {
    		cout << "NO" << '\n';
    		return;
    	}
    	if((c * arr[i - 1] - a * arr[i]) / (b * c - a * d) < 0) {
    		cout << "NO" << '\n';
    		return;
    	}
    	int y = (c * arr[i - 1] - a * arr[i]) / (b * c - a * d);
    	if(arr[i - 1] - b * y < 0 || (arr[i - 1] - b * y) % a) {
    		cout << "NO" << '\n';
    		return;
    	}
    	int x = (arr[i - 1] - b * y) / a;
    	if(yy == -1) {
    		yy = y, xx = x;
    		continue;
    	} else if(y != yy || x != xx) {
    		cout << "NO" <<'\n';
    		return;
    	}
    }
    cout << "YES" << '\n';
}
```


## E. Lost Soul

### 题意

给两个数组 $a, b$ ，有两种操作可以执行无限次：$a_i = b_{i + 1}$ ，或者 $b_i = a_{i + 1}$ 。再进行所有操作前可以选择性执行一次删除一对 $a_i, b_i$ 的操作。求操作后最多有多少对 $a_i = b_i$ 。

### 思路

- 考虑单个数组：
	- xx
	- x_x -> 删除
	- x _  _ x ->跳跃
	-  也就是说 x _ _ ... _ _ x -> 均可以
- 考虑两个数组
	- 下面这种都行：
		- a: x _ ... _ _
		- b: _ _ ... x _ （跳跃或者删除后跳跃）
	- 但是下面这种相邻的不行：
		- a: x _ _
		- b: _ x _

### 示例代码

```cpp
void solve() {
    int n;
    cin >> n;
    vector<int> a(n + 1), b(n + 1);

    map<int, int> mp1, mp2;
    fer(i, 1, n + 1) cin >> a[i];
    fer(i, 1, n + 1) cin >> b[i];

    int ans = 0;
    fer(i, 1, n + 1) {
        if(mp1[a[i]]) ans = max(ans, mp1[a[i]]);
        if(mp2[b[i]]) ans = max(ans, mp2[b[i]]);
        if(a[i] == b[i]) ans = max(ans, i);

        if(mp1[b[i]] && i - mp1[b[i]] != 1) ans = max(ans, mp1[b[i]]);
        if(mp2[a[i]] && i - mp2[a[i]] != 1) ans = max(ans, mp2[a[i]]);

        mp1[a[i]] = i;
        mp2[b[i]] = i;
    }
    cout << ans << '\n';
}
```

