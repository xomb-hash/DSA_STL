# CP Reference Sheet

> **VS Code Preview:** `Cmd+Shift+V` (Mac) or `Ctrl+Shift+V` (Windows/Linux)  
> Use the **Outline panel** (`Cmd+Shift+O`) to jump between sections instantly.

---

## Table of Contents

| # | Section | # | Section |
|---|---------|---|---------|
| 1 | [Mindset & Checklist](#1-mindset--checklist) | 11 | [Monotonic Stack](#11-monotonic-stack) |
| 2 | [Fast I/O & Includes](#2-fast-io--includes) | 12 | [Two Pointers](#12-two-pointers) |
| 3 | [Overflow & Modulo](#3-overflow--modulo) | 13 | [Sliding Window](#13-sliding-window) |
| 4 | [Data Types & Ranges](#4-data-types--ranges) | 14 | [Binary Search](#14-binary-search) |
| 5 | [STL Algorithms](#5-stl-algorithms) | 15 | [Moore's Voting](#15-moores-voting-algorithm) |
| 6 | [Strings](#6-strings) | 16 | [Kadane's Algorithm](#16-kadanes-algorithm) |
| 7 | [Vectors](#7-vectors) | 17 | [Prefix Sum](#17-prefix-sum) |
| 8 | [Stack / Queue / PQ](#8-stack--queue--priority-queue) | 18 | [Math & Number Theory](#18-math--number-theory) |
| 9 | [Set / Map / Multiset](#9-set--map--multiset) | 19 | [Recursion & Backtracking](#19-recursion--backtracking) |
| 10 | [Manual MinHeap](#10-manual-minheap) | 20 | [Trees](#20-trees) |

---

## 1. Mindset & Checklist

> **BEFORE YOU CODE — FORGET HOW TO CODE. THINK, THEN CODE.**

### Pre-Submit Checklist

- [ ] Overflow?
- [ ] `n = 1` case?
- [ ] Empty case?
- [ ] Sorted input assumption?
- [ ] Modulo applied?
- [ ] Index out of bounds?

### General Rules

- Prefer `unordered_map` for frequency
- Use `set` only if sorting is needed
- Avoid manual ASCII math → use `<cctype>`
- Watch iterator invalidation after erase
- **Vectors/deques:** erase shifts elements left — iterators at/after erased position are **invalid**
- **Maps/lists:** node-based — erasing one element does **not** affect other iterators

### Round-off Trick

```cpp
x = round(x * 1e5) / 1e5;
```

---

## 2. Fast I/O & Includes

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
    // code
}
```

### Manual Includes

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_set>
#include <set>
#include <map>
using namespace std;
```

---

## 3. Overflow & Modulo

> ⚠️ **Overflow happens BEFORE modulo is applied.**

```cpp
long long x = a * b;      // ❌ overflow if a, b are int
(long long)(a * b);       // ❌ overflow already happened
1LL * a * b;              // ✅ forces long long multiplication
```

### Mod Operations

```cpp
const int MOD = 1e9 + 7;

(a + b) % MOD = ((a % MOD) + (b % MOD)) % MOD;
(a - b) % MOD = ((a % MOD) - (b % MOD) + MOD) % MOD;  // +MOD forces positive
(a * b) % MOD = (1LL * a * b) % MOD;
(a / b) % MOD = (1LL * a * modInverse(b)) % MOD;
```

### ODD / EVEN Bit Check

```cpp
int result = (n & 1) ? 1 : 0;  // 1 if odd, 0 if even
```

---

## 4. Data Types & Ranges

| Type | Bits | Bytes | Range | ~Magnitude |
|------|------|-------|-------|------------|
| `char` / `int8_t` | 8 | 1 | −128 to 127 | 10² |
| `short` / `int16_t` | 16 | 2 | −32,768 to 32,767 | 10⁴ |
| `int` / `int32_t` | 32 | 4 | −2.1×10⁹ to 2.1×10⁹ | 10⁹ |
| `long long` / `int64_t` | 64 | 8 | −9.2×10¹⁸ to 9.2×10¹⁸ | 10¹⁸ |
| `__int128` | 128 | 16 | ±1.7×10³⁸ | 10³⁸ |
| `float` | 32 | 4 | 6–7 sig digits | ~10³⁸ |
| `double` | 64 | 8 | 15–16 sig digits | ~10³⁰⁸ |

### Lexicographic / ASCII Reference

| Char | Range | Trick |
|------|-------|-------|
| SPACE | 32 | — |
| `0`–`9` | 48–57 | `digit - '0'` |
| `A`–`Z` | 65–90 | `x - 'A'` |
| `a`–`z` | 97–122 | `x - 'a'` |

```cpp
'c' - 'a' = 2  →  2 + 'A' = 'C'   // lowercase letter to its uppercase
```

> Compare strings left to right. First difference decides. Shorter string is smaller if all chars match up to its length.

---

## 5. STL Algorithms

```cpp
#include <algorithm>
```

### Search

```cpp
find(begin, end, x)           // returns iterator, O(n)
binary_search(begin, end, x)  // sorted → bool, O(log n)
lower_bound(begin, end, x)    // iterator to first >= x
upper_bound(begin, end, x)    // iterator to first > x
equal_range(begin, end, x)    // pair of iterators
```

### Count / Sum / Min / Max

```cpp
count(begin, end, x)          // O(n)
accumulate(begin, end, 0)     // sum, O(n)
min(a, b);  max(a, b)
min_element(begin, end)       // returns pointer/iterator
max_element(begin, end)
```

### Sort / Order

```cpp
sort(begin, end)              // O(n log n)
reverse(begin, end)           // O(n)
is_sorted(begin, end)
next_permutation(begin, end)  // O(n)
```

### Remove & Erase

```cpp
// remove() does NOT delete — it shifts elements and returns new logical end
// Erase-remove idiom (vectors):
v.erase(remove(v.begin(), v.end(), x), v.end());

// String: remove all 'a'
s.erase(remove(s.begin(), s.end(), 'a'), s.end());
```

### Permutations

```cpp
sort(a.begin(), a.end());
do {
    for (int x : a) cout << x << " ";
    cout << "\n";
} while (next_permutation(a.begin(), a.end()));
```

### Time Complexities

| Function | Time |
|----------|------|
| `find` | O(n) |
| `count` | O(n) |
| `accumulate` | O(n) |
| `min_element` / `max_element` | O(n) |
| `sort` | O(n log n) |
| `reverse` | O(n) |
| `binary_search` | O(log n) |
| `lower_bound` / `upper_bound` | O(log n) |
| `next_permutation` | O(n) |

---

## 6. Strings

```cpp
s.size()    s.length()
s[i]        s.at(i)
s.pop_back()

// Find & Substr
s.find(x)             // returns index
s.substr(pos, len)    // substring of length len from pos

// Erase
s.erase(pos, count)   // by index
s.erase(it1, it2)     // by iterator range
```

### Char Type Functions

```cpp
isalnum(c)   // alphanumeric?
isdigit(c)   // digit?
tolower(c)   // converts to lowercase  (+32)
toupper(c)   // converts to uppercase  (-32)
```

### Rotate / All Rotations

```cpp
rotate(s.begin(), s.begin() + k, s.end());

// Concatenate with itself to get all rotations:
string doubled = s + s;  // contains every rotation as a substring
```

---

## 7. Vectors

```cpp
vector<int> v;
vector<int> v = {a, b, c};   // ✅
// vector<int> v[3] = {a, b, c};  ❌ wrong

v.push_back(x)      v.emplace_back(x)  // emplace_back is faster
v.pop_back()        v.clear()
v.begin()           v.end()
v.back()            v.empty()
v.size()            v.swap(v2)

v.erase(it)                            // single element
v.erase(begin, end)                    // range
v.insert(v.begin() + index, value)    // no index-based insert directly
```

### Iterator Invalidation

```cpp
// After erase:
it = v.erase(it);   // ✅ it now points to next valid element

auto it2 = v.begin() + 1;
v.erase(it2);
cout << *it2;       // ❌ INVALID — element is gone
```

---

## 8. Stack / Queue / Priority Queue

### Stack

> ⚠️ Stack is **NOT** iterable — no range-based for loop!

```cpp
stack<int> s;
s.push(a)    s.emplace(a)   // NOT emplace_back
s.top()
s.size()
s.pop()     // ALWAYS check !s.empty() before pop
s.empty()
```

### Queue

```cpp
queue<int> q;
q.push(1)    q.emplace(a)
q.front()    q.back()
q.pop()      // check !q.empty() first
q.empty()
```

### Priority Queue

```cpp
priority_queue<int> pq;                                    // max at top (default)
priority_queue<int, vector<int>, greater<int>> pq_min;     // min at top

pq.push(a)    pq.emplace(a)
pq.top()
pq.pop()
```

---

## 9. Set / Map / Multiset

### Set

```cpp
set<int> st;
unordered_set<int> us;   // O(1) avg, random order

st.insert(1)     st.emplace(1)
st.size()
st.begin()       st.end()
st.find(x)       // iterator, or st.end() if not found
st.erase(it)     // by iterator
st.erase(st.find(2), st.find(4))  // range [)
st.count(x)      // 0 or 1
st.lower_bound(x)   // >= x
st.upper_bound(x)   // > x
```

### Multiset

```cpp
multiset<int> mst;

mst.insert(1)
mst.count(x)                     // counts ALL occurrences
auto it = mst.find(2);           // points to FIRST occurrence
mst.erase(mst.find(x));         // ✅ remove ONE copy
mst.erase(x);                    // ⚠️ removes ALL copies
mst.upper_bound(x)
mst.lower_bound(x)
```

### Map

```cpp
map<int, int> mp;
map<int, int> mp = {{1,10}, {2,20}};   // stores in key order

mp[key]                   // O(log n) — creates key if missing!
mp.insert({k, v})         // O(log n)
mp.find(x)                // iterator or mp.end()
mp.count(key)             // 0 or 1
mp.erase(key or it)
mp.clear()
mp.lower_bound(x)    mp.upper_bound(x)

// Iterate
for (auto [k, v] : mp) { ... }       // structured binding
for (auto& p : mp) { p.first; p.second; }
```

### Unordered Map

```cpp
unordered_map<int, int> mp;   // O(1) avg, O(n) worst

mp[key]
mp.find(x)    mp.count(key)
mp.erase(key or it)
mp.clear()
```

> Use `unordered_map` for frequency counting. Use `map` when key order matters.

### Check Unique Values in Map

```cpp
bool areValuesUnique(map<int, int>& mp) {
    set<int> seen;
    for (auto& p : mp) {
        if (seen.count(p.second)) return false;
        seen.insert(p.second);
    }
    return true;
}
```

---

## 10. Manual MinHeap

```cpp
class MinHeap {
    vector<int> h;
public:
    void insert(int x) {
        h.push_back(x);
        int i = h.size() - 1;
        while (i > 0 && h[(i-1)/2] > h[i]) {
            swap(h[i], h[(i-1)/2]);
            i = (i-1)/2;
        }
    }

    int getMin() { return h[0]; }

    void heapify(int i) {
        int s = i, l = 2*i+1, r = 2*i+2;
        if (l < h.size() && h[l] < h[s]) s = l;
        if (r < h.size() && h[r] < h[s]) s = r;
        if (s != i) { swap(h[i], h[s]); heapify(s); }
    }

    int extractMin() {
        int root = h[0];
        h[0] = h.back();
        h.pop_back();
        heapify(0);
        return root;
    }

    void decreaseKey(int i, int val) {
        h[i] = val;
        while (i > 0 && h[(i-1)/2] > h[i]) {
            swap(h[i], h[(i-1)/2]);
            i = (i-1)/2;
        }
    }

    void deleteKey(int i) {
        decreaseKey(i, INT_MIN);
        extractMin();
    }
};
```

---

## 11. Monotonic Stack

> Used for: **Next Greater / Smaller Element**. Sometimes push index instead of value.

```cpp
// Increasing stack (pop when top is greater)
while (!st.empty() && st.top() > a[i])
    st.pop();

// Decreasing stack (pop when top is smaller)
while (!st.empty() && st.top() < a[i])
    st.pop();
```

### Template

```cpp
for (int i = 0; i < n; i++) {
    while (!st.empty() && condition)
        st.pop();
    st.push(a[i]);   // or push index i
}
// Time: O(n)
```

---

## 12. Two Pointers

> Pointers are **ROLES**, not just indices. Fix the role and never change it.

```cpp
while (l < r) {
    if (condition) l++;
    else r--;
}
```

### Three Pointers — Dutch Flag

```
0 region: [0 → low-1]
1 region: [low → mid-1]
unknown:  [mid → high]
2 region: [high+1 → n-1]
```

### Rotate Array by K

```cpp
reverse(nums.begin(), nums.end());          // step 1: reverse all
reverse(nums.begin(), nums.begin() + k);    // step 2: reverse first k
reverse(nums.begin() + k, nums.end());      // step 3: reverse rest
```

---

## 13. Sliding Window

```cpp
int l = 0;
while (r < n) {
    // add element at r

    while (/* condition breaks */) {
        // remove element at l
        l++;
    }

    r++;
}
```

---

## 14. Binary Search

> Binary search is about **ELIMINATION** — reduce the search space, not just finding an element.

```cpp
while (l <= r) {
    int mid = l + (r - l) / 2;   // avoids overflow vs (l+r)/2

    if (/* condition */)
        r = mid - 1;
    else
        l = mid + 1;
}
```

### Search in Rotated Array

```cpp
// Key: identify which half is sorted, then check if target is in that range
if (nums[low] == nums[mid] && nums[high] == nums[mid]) {
    low++; high--;  // shrink search space for duplicates [3,3,3,3,3]
} else if (nums[mid] >= nums[low]) {
    // left half is sorted
    if (nums[low] <= target && target < nums[mid]) high = mid - 1;
    else low = mid + 1;
} else {
    // right half is sorted
    if (nums[mid] < target && target <= nums[high]) low = mid + 1;
    else high = mid - 1;
}
```

---

## 15. Moore's Voting Algorithm

> Finds majority element in **O(n) time, O(1) space**.  
> Majority element = appears more than n/2 times.

```cpp
int el = 0, cnt = 0;
for (int i : nums) {
    if (cnt == 0) { el = i; cnt = 1; }
    else if (i == el) cnt++;
    else cnt--;
}
// el is the candidate — verify separately if needed
```

---

## 16. Kadane's Algorithm

> Max sum subarray. A negative running sum only hurts — reset to 0.

```cpp
int curr_sum = nums[0], max_sum = nums[0];
for (int i = 1; i < n; i++) {
    curr_sum = max(nums[i], curr_sum + nums[i]);
    max_sum  = max(max_sum, curr_sum);
}
```

### With Index Tracking

```cpp
int sum = 0, maxi = INT_MIN, start = 0, end = 0, s = 0;
for (int i = 0; i < n; i++) {
    if (sum == 0) s = i;
    sum += nums[i];
    if (sum > maxi) { maxi = sum; start = s; end = i; }
    if (sum < 0) sum = 0;
}
```

---

## 17. Prefix Sum

> Know sum(1→5) and sum(1→8) → compute sum(5→8) in O(1).

```cpp
vector<int> pre(n);
pre[0] = arr[0];
for (int i = 1; i < n; i++)
    pre[i] = pre[i-1] + arr[i];

// Range sum [l, r]:
int rangeSum = pre[r] - (l > 0 ? pre[l-1] : 0);
```

> For subarray sum = k: check if prefix exists BEFORE adding current element to map.

### Prefix GCD

```cpp
prefix[0] = arr[0];
for (int i = 1; i < n; i++)
    prefix[i] = gcd(prefix[i-1], arr[i]);
```

### MEX

```cpp
int mex(vector<int>& a) {
    unordered_set<int> s(a.begin(), a.end());
    int x = 0;
    while (s.count(x)) x++;
    return x;
}
```

### Gap Method (Merge Two Sorted Arrays In-Place)

```cpp
// Treat as one virtual array of size m+n
// Start gap = ceil((m+n)/2), halve each round

int len = m + n;
for (int gap = (len+1)/2; gap > 0; gap = (gap == 1) ? 0 : (gap+1)/2) {
    for (int l = 0, r = l + gap; r < len; l++, r++) {
        if (l < n && r >= n)        swap_if_greater(a[l], b[r-n]);
        else if (l >= n)            swap_if_greater(b[l-n], b[r-n]);
        else                        swap_if_greater(a[l], a[r]);
    }
}
```

### Generate All Subsets

```cpp
void findSubset(int ind, vector<int>& nums,
                vector<int> ds, vector<vector<int>>& ans) {
    if (ind == nums.size()) { ans.push_back(ds); return; }
    ds.push_back(nums[ind]);
    findSubset(ind+1, nums, ds, ans);   // take
    ds.pop_back();
    findSubset(ind+1, nums, ds, ans);   // not take
}
// Call: findSubset(0, nums, {}, ans);
```

---

## 18. Math & Number Theory

### GCD & LCM

```cpp
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
int lcm(int a, int b) { return a / gcd(a, b) * b; }
```

### Count Divisors — sqrt approach

```cpp
int countDivisors(int n) {
    int cnt = 0;
    for (int i = 1; i*i <= n; i++) {
        if (n % i == 0)
            cnt += (i == n/i) ? 1 : 2;
    }
    return cnt;
}
// Formula: n = p1^a * p2^b * p3^c  →  divisors = (a+1)(b+1)(c+1)
```

### Modular Inverse (Fermat's Little Theorem)

```cpp
// Requires MOD to be prime and gcd(a, MOD) = 1
// modInverse(a) = a^(MOD-2) % MOD  — use fast exponentiation
```

### Fast Exponentiation

```cpp
long long fastPow(long long x, long long n, long long MOD) {
    long long ans = 1;
    x %= MOD;
    while (n > 0) {
        if (n % 2 != 0) { ans = ans * x % MOD; n--; }
        else             { x = x * x % MOD;     n /= 2; }
    }
    return ans;
}
```

### Sieve of Eratosthenes

```cpp
vector<int> sieve(int n) {
    vector<int> nums(n+1, 1);
    nums[0] = nums[1] = 0;
    for (int i = 2; i*i <= n; i++) {
        if (nums[i])
            for (int j = i*i; j <= n; j += i)
                nums[j] = 0;
    }
    return nums;  // nums[i] == 1 means i is prime
}
// Time: O(n log log n)
```

### nCr

```cpp
long long nCr(int n, int r) {
    if (r > n) return 0;
    if (r == 0 || r == n) return 1;
    if (r > n - r) r = n - r;   // symmetry: nCr = nC(n-r)
    long long ans = 1;
    for (int i = 1; i <= r; i++)
        ans = ans * (n - i + 1) / i;
    return ans;
}
```

### Precompute Factorials

```cpp
const int N = 100000;
const int MOD = 1e9 + 7;
long long fact[N+1];

void precompute() {
    fact[0] = 1;
    for (int i = 1; i <= N; i++)
        fact[i] = (fact[i-1] * i) % MOD;
}
```

### Log & Bit Tricks

```cpp
1 + (int)log10(x)    // number of digits in x
(int)log2(n)         // index of highest set bit

// XOR tricks:
a ^ a = 0            // cancels itself
a ^ 0 = a            // identity

// pow() is NOT for large integers — use fastPow()
```

---

## 19. Recursion & Backtracking

> **DRAW THE RECURSION TREE — it has all the hints.**

### Rules

- If increasing index toward base → base case is `n`
- If decreasing → base case is `0`
- A function must have **structural consistency**: `f(n-1)` should be similar in structure to `f(n)`
- You can replace loops or two pointers with recursion (palindrome, reverse array)

### Take or Not Take (Subsets / Subsequences)

```cpp
// Pattern 1: enumerate all subsets
ds.push_back(arr[i]);
f(index + 1, ds);     // take
ds.pop_back();
f(index + 1, ds);     // not take

// Pattern 2: return true/false for first valid subsequence
ds.push_back(arr[i]);
if (f(index+1, ds)) return true;
ds.pop_back();
if (f(index+1, ds)) return true;
return false;
```

### Reverse Array Recursively

```cpp
void reverseArr(int begin, vector<int>& arr) {
    int n = arr.size();
    if (begin >= n/2) return;
    swap(arr[begin], arr[n-begin-1]);
    reverseArr(begin + 1, arr);
}
// Call: reverseArr(0, arr);
```

### Palindrome Check (Recursive)

```cpp
bool palindrome(int i, vector<int>& arr) {
    int n = arr.size();
    if (i >= n/2) return true;
    if (arr[i] != arr[n-i-1]) return false;
    return palindrome(i + 1, arr);
}
```

### Permutations — Frequency Map

```cpp
void permutate(vector<int>& nums, vector<vector<int>>& ans,
               vector<int> ds, unordered_map<int,int> used) {
    if (ds.size() == nums.size()) { ans.push_back(ds); return; }
    for (int i = 0; i < nums.size(); i++) {
        if (!used[nums[i]]) {
            ds.push_back(nums[i]);   // do
            used[nums[i]] = 1;
            permutate(nums, ans, ds, used);  // explore
            ds.pop_back();           // undo
            used[nums[i]] = 0;
        }
    }
}
```

### Permutations — Swap-Based (No Extra Memory)

```cpp
void permutate2(int ind, vector<vector<int>>& ans, vector<int>& nums) {
    if (ind == nums.size()-1) { ans.push_back(nums); return; }
    for (int i = ind; i < nums.size(); i++) {
        swap(nums[ind], nums[i]);
        permutate2(ind+1, ans, nums);
        swap(nums[ind], nums[i]);
    }
}
```

---

## 20. Trees

### Binary Tree Types

| Type | Rule |
|------|------|
| Full Binary Tree | Every node has 0 or 2 children |
| Complete Binary Tree | All levels filled; last level filled left to right |
| Perfect Binary Tree | All internal nodes have 2 children; all leaves at same level |

> **Depth** = edges from root to node (root depth = 0)  
> **Height** = edges from node to its deepest leaf

> In linked lists: **always use a dummy node** to avoid null head edge cases.

### Inorder Traversal

```cpp
void inorder(TreeNode* root, vector<int>& store) {
    if (!root) return;
    inorder(root->left, store);
    store.push_back(root->val);
    inorder(root->right, store);
}
```

### Level Order Traversal (BFS)

```cpp
// Pattern: process node → push children
vector<vector<int>> levelOrder(TreeNode* root) {
    if (!root) return {};
    queue<TreeNode*> q;
    vector<vector<int>> res;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();
        vector<int> level;
        for (int i = 0; i < sz; i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left)  q.push(node->left);
            if (node->right) q.push(node->right);
        }
        res.push_back(level);
    }
    return res;
}
```

### Height of Complete Binary Tree

```cpp
int getLeftHeight(TreeNode* root) {
    int h = 0;
    while (root) { h++; root = root->left; }
    return h;
}
```

### Sum Root to Leaf Nodes

```cpp
// Pass down: sum = sum * 10 + root->val
```

### Palindrome Number

```cpp
bool isPalindrome(int x) {
    if (x < 0 || (x % 10 == 0 && x != 0)) return false;
    int reversed = 0;
    while (x > reversed) {
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }
    return x == reversed || x == reversed / 10;
}
```

---

*End of Reference Sheet*
