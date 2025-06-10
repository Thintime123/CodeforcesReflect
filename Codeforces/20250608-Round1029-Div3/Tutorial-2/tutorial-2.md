## C

### Target

将一个数组分为尽可能多的子数组（有序），使得后一个子数组包含前一个所有元素

### Thinking

1. 记录所有数的个数，便于记录终止项
2. 记录当前子数组还需满足的条件（已经有的和还需满足的）
3. 可以用`unordered_set`提速




### Code

```c++
#include <bits/stdc++.h>
#define ll long long
#define fer(i, m, n) for (ll i = m; i < n; i++)
using namespace std;

void solve() {
    int n; 
    cin >> n;
    vector<int> a(n);
    for (int &x : a) 
        cin >> x;

    vector<int> freq(n + 1, 0); 
    for (int x : a) 
        ++freq[x];

    unordered_set<int> req, cur, emts;
    int result = 0;

    fer(i, 0, n) {
        int v = a[i];
        
        --freq[v];             
        cur.insert(v);     

        if (freq[v] == 0) 
            emts.insert(v); 

        if (req.erase(v)) ;   

        
        if (i != n - 1 && req.empty() && emts.empty()) {
            ++result;             
            req.swap(cur); 
            cur.clear();        
            emts.clear(); 
        }
    }
    
    ++result; 
    cout << result << '\n';
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0), cout.tie(0);
    int t = 1;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}

```

## D

### Target

判断一个数组(大小为`n`)是否可以由若干`1,2,...,n` 和 `n,...,2,1`相加而得

### Thinking

1. 显然它们必然是 **等差数列**

2. $$
   a_i = l \cdot i + k \cdot (n - i + 1) \quad \text{对所有 } i = 1..n
   $$

3. $$
   a_i = (l - k) \cdot i + k \cdot (n + 1)
   $$

4. $$
   a_1 = d + k(n + 1) \Rightarrow k = \frac{a_1 - d}{n + 1}
   $$

5. 相应条件满足即可

### Code

```c++
#include <bits/stdc++.h>
#define ll long long
#define fer(i, m, n) for (ll i = m; i < n; i++)
using namespace std;

void solve() {
    int n;
    cin >> n;
    vector<ll> a(n);
    for (ll &x : a) cin >> x;

    
    ll d = a[1] - a[0];
    for (int i = 2; i < n; i++) {
        if (a[i] - a[i-1] != d) {
            cout << "NO\n";
            return;
        }
    }

    
    ll c = a[0] - d;
    ll m = n + 1;
    if (c < 0 || c % m != 0) {
        cout << "NO\n";
        return;
    }
    ll l = c / m;        
    ll k = d + l;        

    
    if (k >= 0 && l >= 0) cout << "YES\n";
    else                 cout << "NO\n";
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```

## E - TODO

### Target

### Thinking

### Code



