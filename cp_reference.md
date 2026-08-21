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
***************************************THOSE WHO CANNOT REMEMBER THE PAST ARE CONDEMNED TO REPEAT IT****************************************************


# 🧩 Mastering Dynamic Programming

> **"Master the recursion. Recognize the pattern. Build the DP."**

A structured, long-term reference for learning Dynamic Programming the right way — not by memorizing problems, but by mastering the **thinking process** that derives every DP solution from first principles.

---

## 📌 Table of Contents

- [🎯 Introduction](#-introduction)
- [🧠 What is Dynamic Programming?](#-what-is-dynamic-programming)
- [🔁 Why Recursion Comes First](#-why-recursion-comes-first)
- [🧱 Recursion Fundamentals Required for DP](#-recursion-fundamentals-required-for-dp)
- [🛠️ The DP Problem-Solving Framework](#️-the-dp-problem-solving-framework)
- [📦 State Definition](#-state-definition)
- [🔀 Core Recursive Patterns](#-core-recursive-patterns)
  - [Pattern 1 — Take / Not Take](#pattern-1--take--not-take)
  - [Pattern 2 — Try All Possible Next Choices](#pattern-2--try-all-possible-next-choices)
  - [Pattern 3 — Min / Max Over Choices](#pattern-3--min--max-over-choices)
  - [Pattern 4 — Count the Number of Ways](#pattern-4--count-the-number-of-ways)
  - [Pattern 5 — Boolean / Possible](#pattern-5--boolean--possible)
  - [Pattern 6 — Grid DP](#pattern-6--grid-dp)
  - [Pattern 7 — Partition DP](#pattern-7--partition-dp)
  - [Pattern 8 — Interval DP](#pattern-8--interval-dp)
- [🧮 Min / Max / Count / Boolean Cheat Sheet](#-min--max--count--boolean-cheat-sheet)
- [🔺 Recursion → Memoization](#-recursion--memoization)
- [🔻 Memoization → Tabulation](#-memoization--tabulation)
- [📉 Tabulation → Space Optimization](#-tabulation--space-optimization)
- [🗂️ Major DP Categories](#️-major-dp-categories)
- [🗺️ DP Learning Roadmap](#️-dp-learning-roadmap)
- [🔍 Pattern Recognition Cheat Sheet](#-pattern-recognition-cheat-sheet)
- [🧭 Universal DP Template](#-universal-dp-template)
- [💻 Reusable C++ Templates](#-reusable-c-templates)
- [📋 Problem Tracker](#-problem-tracker)
- [✅ Pattern Mastery Tracker](#-pattern-mastery-tracker)
- [⚠️ Common Mistakes](#️-common-mistakes)
- [❓ Questions to Ask When Stuck](#-questions-to-ask-when-stuck)
- [🏆 Golden Rules](#-golden-rules)
- [🧭 Final Mental Model](#-final-mental-model)

---

## 🎯 Introduction

This repository exists for one purpose: **to master Dynamic Programming by understanding it, not by memorizing it.**

DP has a reputation for being intimidating. The truth is that DP is not a separate skill from recursion — it **is** recursion, plus the observation that some recursive calls repeat, plus a way to avoid redoing that work. If you can write correct recursion for a problem, you are one short step away from a working DP solution.

**Why I'm building this repository:**
- To stop pattern-matching problems to memorized solutions
- To build the ability to **derive** a DP solution for a problem I have never seen before
- To understand *why* a recurrence works, not just *that* it works
- To build a durable, reusable mental framework instead of a pile of disconnected code snippets

> **Philosophy:**
> Don't memorize DP solutions.
> Understand the recursive pattern and derive the DP.

### The Learning Pipeline

Every DP problem — no exceptions — flows through this pipeline:

```
                Problem
                   │
                   ▼
                 State
                   │
                   ▼
                Choices
                   │
                   ▼
               Recursion
                   │
                   ▼
       Overlapping Subproblems
                   │
                   ▼
              Memoization
                   │
                   ▼
              Tabulation
                   │
                   ▼
          Space Optimization
```

Every section in this repository maps to one node in that pipeline. Skipping a node is how people end up "memorizing" DP instead of understanding it.

---

## 🧠 What is Dynamic Programming?

**Dynamic Programming (DP)** is a technique for solving problems by breaking them into smaller subproblems, solving each subproblem once, and reusing the stored result instead of recomputing it.

DP works when a problem has two properties:

| Property | Meaning |
|---|---|
| **Overlapping Subproblems** | The same smaller subproblem is solved multiple times during recursion |
| **Optimal Substructure** | The optimal solution to the problem can be built from optimal solutions to its subproblems |

If a problem has both properties, we can trade **repeated computation** for **stored computation** — that trade *is* Dynamic Programming.

### Example: Fibonacci

```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```

### The Recursion Tree

```
                         fib(5)
                        /       \
                  fib(4)         fib(3)
                 /      \        /      \
            fib(3)     fib(2) fib(2)   fib(1)
           /     \      /   \   /   \
      fib(2)  fib(1) fib(1) fib(0) fib(1) fib(0)
      /    \
   fib(1) fib(0)
```

Look closely — `fib(3)` is computed **2 times**, `fib(2)` is computed **3 times**, `fib(1)` is computed **5 times**. These repeated calls are the *overlapping subproblems*. Plain recursion recomputes them from scratch every time; DP stores the result the first time and reuses it.

### Normal Recursion vs Memoization vs Tabulation

| Approach | Direction | Storage | Speed |
|---|---|---|---|
| **Normal Recursion** | Top-down | None | Exponential — recomputes everything |
| **Memoization** | Top-down | Cache (array/map) | Polynomial — each state computed once |
| **Tabulation** | Bottom-up | Table (array) | Polynomial — no recursion/call-stack overhead |

---

## 🔁 Why Recursion Comes First

This is one of the **most important sections** in this repository.

You cannot skip recursion and jump straight to DP. Recursion **is** the decision-making structure of the problem. DP is just recursion with memory. If your recursion is wrong, your DP will be wrong — memoizing a broken recurrence just makes a broken answer fast.

### Example: House Robber

At every house, you face a decision:

```
                    House i
                   /        \
                Take        Skip
                 │            │
               i+2          i+1
```

- **Take** house `i` → you gain `nums[i]`, but now you can't touch house `i-1`, so you jump to `i-2`
- **Skip** house `i` → you gain nothing here, but you're free to consider house `i-1`

This decision tree **is** the recurrence:

```cpp
f(i) = max( nums[i] + f(i - 2),   // take
                       f(i - 1) )  // skip
```

Notice: we didn't "invent" this formula. We simply **translated the recursion into an equation.** This is the core skill of DP.

> **If you understand the recursion, DP becomes much easier.**
> DP does not require new thinking — it requires recognizing that your recursive tree has repeated branches, and storing their answers.

---

## 🧱 Recursion Fundamentals Required for DP

Before attempting DP, be completely comfortable with the following recursive concepts.

### 1. What a Recursive Function Means
A recursive function answers a smaller version of the same question it's being asked, then combines that smaller answer into the final one.

### 2. State
The **state** is the set of parameters that fully describe "where you are" in the problem.

```cpp
int f(int i) { ... }        // state = i
int f(int i, int j) { ... } // state = (i, j)
```

### 3. Base Case
The smallest version of the problem, answered directly without further recursion.

```cpp
if (n == 0) return 0; // base case
```

### 4. Recursive Case
The general rule that reduces the current problem to a smaller one.

```cpp
return f(n - 1) + f(n - 2);
```

### 5. Choices
At each step, what options do you have? Recursion is essentially "try every choice, recurse on each."

### 6. Return Value
What does `f(...)` actually represent? Always be able to state this in one sentence — e.g., *"f(i) = the maximum money that can be robbed from houses 0 to i."*

### 7. Recursion Tree
The branching diagram of all calls made. Drawing this by hand for small inputs is the single best debugging tool for DP.

### 8. Call Stack
Each recursive call is pushed onto the stack until a base case is hit, then results are popped back up ("unwound"). Stack depth = recursion depth.

### 9. Overlapping Recursive Calls
When the *same* `(state)` is reached via different paths in the tree. This is the signal that memoization will help.

### 10. Time Complexity of Recursion
Without memoization: roughly `(number of choices) ^ (depth)`. With memoization: `(number of unique states) × (work per state)`.

### Small C++ Example — Tracing by Hand

```cpp
int f(int n) {
    if (n == 0) return 0;          // base case
    return n + f(n - 1);           // recursive case
}
```

To trace: write out each call on paper as `f(3) → 3 + f(2) → 3 + (2 + f(1)) → 3 + (2 + (1 + f(0))) → 3 + (2 + (1 + 0)) = 6`. Always trace by expanding **inward** then collapsing **outward**.

---

## 🛠️ The DP Problem-Solving Framework

Every DP problem, without exception, should be approached in this order:

| Step | Action | Ask Yourself |
|---|---|---|
| 1 | **Define the state** | What parameters fully describe this subproblem? |
| 2 | **Identify the choices** | What decisions can I make from here? |
| 3 | **Write the recurrence** | How does the answer depend on smaller states? |
| 4 | **Identify the base case** | What's the smallest input I can answer directly? |
| 5 | **Classify the problem** | Min? Max? Counting? Boolean? |
| 6 | **Write pure recursion** | Does it give the correct answer (ignore speed)? |
| 7 | **Identify overlapping subproblems** | Are the same states being recomputed? |
| 8 | **Add memoization** | Store each state's answer the first time it's computed |
| 9 | **Convert to tabulation** | Rebuild bottom-up in dependency order |
| 10 | **Optimize space** | Do I really need the whole table, or just the last few rows? |

### Example Walkthrough — Climbing Stairs

1. **State**: `f(i)` = number of ways to reach step `i`
2. **Choices**: from step `i`, you could have arrived via a 1-step or 2-step jump
3. **Recurrence**: `f(i) = f(i-1) + f(i-2)`
4. **Base case**: `f(0) = 1`, `f(1) = 1`
5. **Classification**: Counting DP (asks "how many ways")
6. **Pure recursion**: implement directly from the recurrence
7. **Overlapping subproblems**: yes — `f(i-1)` is recomputed by multiple branches
8–10. Memoize → tabulate → space-optimize (see later sections for the mechanics)

---

## 📦 State Definition

> **"If two situations can have different future answers, the state must be able to distinguish them."**

A **DP state** is the minimal set of variables that uniquely determines the answer to a subproblem. Getting the state right is the single highest-leverage skill in DP — a wrong or incomplete state guarantees a wrong answer, no matter how clean your code is.

### Common State Shapes

| State | Typical Meaning |
|---|---|
| `f(i)` | Answer considering only the first `i` elements |
| `f(i, j)` | Answer comparing/combining two sequences up to indices `i` and `j` |
| `f(i, capacity)` | Answer at item `i` with `capacity` resource remaining (knapsack-style) |
| `f(row, col)` | Answer at a grid cell |
| `f(l, r)` | Answer over the interval/segment `[l, r]` |
| `f(node, state)` | Answer at a tree/graph node, carrying extra context (e.g., "parent included?") |

### Correct vs Incorrect State — Example

**Problem**: Longest subsequence where you can pick at most `k` elements from an array, maximizing sum.

- ❌ **Incomplete state**: `f(i)` — only tracks position, but two paths reaching index `i` might have used a *different number of picks so far*, which changes what's still allowed. Two situations at the same `i` can have different future answers, and this state can't tell them apart.
- ✅ **Correct state**: `f(i, count)` — tracks both position **and** how many elements have been picked, since both affect what happens next.

**Rule of thumb**: List everything that changes as the recursion proceeds. If removing a variable from your state causes two genuinely different situations to collide into the same `f(...)` call, that variable belongs in the state.

---

## 🔀 Core Recursive Patterns

Almost every DP problem is a variation of a small set of recursive shapes. Learn these shapes, not individual problems.

### Pattern 1 — Take / Not Take

```
              Current Element
                 /       \
              Take       Skip
```

At each element, decide whether to include it in the solution or not. This single pattern powers a huge fraction of all DP problems.

- **Take**: use the current element, move to the next *reduced* state
- **Skip**: ignore the current element, move to the next state unchanged

This naturally produces:
```
max(...)   → optimization problems
min(...)   → optimization problems
count(...) → counting problems
bool(...)  → feasibility problems
```

**Examples**: House Robber · 0/1 Knapsack · Subset Sum · Partition Equal Subset Sum · Target Sum

**Generic C++ Template**:
```cpp
int f(int i, /* other state */) {
    if (/* base case */) return baseValue;

    int take = /* value if we take */ + f(i - 1 /* or reduced state */);
    int skip = f(i - 1);

    return max(take, skip); // or min, or take + skip for counting, or take || skip for boolean
}
```

---

### Pattern 2 — Try All Possible Next Choices

Some problems don't have a binary take/skip choice — from the current state, you can jump to *many* possible next states, and you must try all of them.

```
Current State
   → Choice 1
   → Choice 2
   → Choice 3
   → ...
```

```cpp
for (int j = i + 1; j < n; j++) {
    // try transitioning from i to j
}
```

**Examples**: Jump Game variants · Partition problems · Interval-based problems

**Generic C++ Template**:
```cpp
int f(int i) {
    if (/* base case */) return baseValue;

    int best = /* worst possible value */;
    for (int j = i + 1; j <= n; j++) {
        best = optFunc(best, cost(i, j) + f(j));
    }
    return best;
}
```

---

### Pattern 3 — Min / Max Over Choices

Pure optimization DP: try every available choice, and take the best (or worst) result.

```cpp
min(choice1, choice2, ...)
max(choice1, choice2, ...)
```

**Examples**: Frog Jump · Minimum Cost Climbing Stairs · Minimum Path Sum · Maximum Path Sum · Maximum Profit

**Generic C++ Template**:
```cpp
int f(int i) {
    if (i == 0) return 0;
    if (i == 1) return cost(1);

    int option1 = cost(i) + f(i - 1);
    int option2 = cost(i) + f(i - 2);

    return min(option1, option2); // or max, depending on the problem
}
```

---

### Pattern 4 — Count the Number of Ways

When the question asks **"how many ways?"**, we generally **add** the number of ways contributed by each choice rather than taking a min/max.

```cpp
f(n) = f(n - 1) + f(n - 2);
```

**Examples**: Climbing Stairs · Unique Paths · Decode Ways · Coin Change II

Counting DP answers frequently need to be taken **modulo** a large prime (commonly `1e9 + 7`) to avoid overflow, since the number of ways can grow exponentially:

```cpp
const int MOD = 1e9 + 7;
dp[i] = (dp[i - 1] + dp[i - 2]) % MOD;
```

---

### Pattern 5 — Boolean / Possible

The answer is `true` or `false` — "is it possible to...?"

```cpp
choice1 || choice2
```

**Examples**: Subset Sum · Partition Equal Subset Sum · Word Break · Target-sum-related problems

**Generic C++ Template**:
```cpp
bool f(int i, int target) {
    if (target == 0) return true;
    if (i == 0 || target < 0) return false;

    bool notTake = f(i - 1, target);
    bool take = f(i - 1, target - arr[i]);

    return notTake || take;
}
```

---

### Pattern 6 — Grid DP

State: `f(row, col)`.

```
        UP
        ↑
        |
LEFT ← (i,j)
```

You typically move from one or two neighboring cells (up/left, or up/down/left/right depending on the problem), with boundary conditions at row 0 / column 0.

Grid DP can be **counting** (number of paths), **min/max** (cheapest or most valuable path), or occasionally **boolean**.

**Examples**: Unique Paths · Unique Paths II · Minimum Path Sum · Triangle · Maximum Path Sum

---

### Pattern 7 — Partition DP

"Try every partition point" — split a range at every possible position and combine the results of the two halves.

```
[l ------------------------- r]

             k
             ↓

[l -------- k] [k+1 -------- r]
```

```cpp
for (int k = l; k < r; k++) {
    // combine result of [l, k] and [k+1, r]
}
```

**Examples**: Matrix Chain Multiplication · Palindrome Partitioning · Minimum Cost to Cut a Stick · Boolean Parenthesization

---

### Pattern 8 — Interval DP

State: `f(l, r)` — the state itself represents a **range**, not a single index.

```
[l ---------------------- r]
          |
          k
```

Interval DP typically tries every split point `k` inside `[l, r]` and combines the cost of the two resulting sub-intervals, often with an added "merge cost."

**Examples**: Matrix Chain Multiplication · Burst Balloons · Palindrome Partitioning · Minimum Cost to Cut a Stick · Optimal BST

---

## 🧮 Min / Max / Count / Boolean Cheat Sheet

The wording of a problem almost always tells you what operator sits at the top of your recurrence.

```
MINIMUM  →  min()
MAXIMUM  →  max()
COUNT    →  +
BOOLEAN  →  ||
```

| Question type | Operator | Example |
|---|---|---|
| "What is the **minimum** cost/steps/path...?" | `min()` | Minimum Path Sum |
| "What is the **maximum** value/profit/sum...?" | `max()` | House Robber |
| "**How many ways** can you...?" | `+` | Climbing Stairs |
| "**Is it possible** to...?" | `\|\|` | Subset Sum |

Before writing a single line of recursion, read the problem statement and classify it using this table. This alone eliminates a huge class of mistakes.

---

## 🔺 Recursion → Memoization

Memoization is **recursion + a cache**. Nothing else changes — the recursive logic stays identical.

### Example Transformation

**Pure recursion:**
```cpp
int f(int i) {
    if (i <= 1) return i;
    return f(i - 1) + f(i - 2);
}
```

**Memoized:**
```cpp
int dp[MAXN];
memset(dp, -1, sizeof(dp)); // initialize once, outside f

int f(int i) {
    if (i <= 1) return i;

    if (dp[i] != -1) return dp[i]; // return cached answer if we've solved this state before

    return dp[i] = f(i - 1) + f(i - 2); // solve, store, and return
}
```

Only two additions were needed:
1. A **cache check** at the top: if this state was already solved, return it immediately.
2. A **store step**: before returning, save the answer into the cache.

### Why Memoization Works
Because each unique state now gets computed **exactly once**. All subsequent calls to that state are O(1) lookups instead of full re-recursion.

- **Top-down DP**: you still think and call functions the "recursive" way — starting from the big problem and breaking it down.
- **State storage**: the cache is indexed exactly by the state — `dp[i]` for a 1D state, `dp[i][j]` for a 2D state, etc.
- **Complexity improvement**: from exponential (`O(2^n)` for Fibonacci-style recursion) down to `O(number of states × work per state)`.

---

## 🔻 Memoization → Tabulation

**Top-down (memoization)** starts from the big problem and recurses down.
**Bottom-up (tabulation)** starts from the base cases and builds up, filling the table in an order where every dependency is already computed.

### How to Determine Iteration Order

Look at what each state **depends on**:

- If `dp[i]` depends on `dp[i-1]` and `dp[i-2]` (depends on *smaller* indices) → iterate **forward**: `for (int i = 2; i <= n; i++)`
- If `dp[i]` depends on `dp[i+1]` (depends on *larger* indices) → iterate **backward**: `for (int i = n - 1; i >= 0; i--)`
- If `dp[i][j]` depends on `dp[i-1][j]`, `dp[i][j-1]`, and `dp[i-1][j-1]` → iterate rows top-to-bottom, columns left-to-right
- If `dp[l][r]` depends on `dp[l][k]` and `dp[k+1][r]` for `k` between `l` and `r` (interval shrinks inward) → iterate by **increasing interval length**

**Rule**: tabulation always fills the table in an order such that, by the time you compute `dp[state]`, every state it depends on has already been filled in.

### Example — Climbing Stairs

**Memoized (top-down):**
```cpp
int dp[MAXN];
int f(int i) {
    if (i <= 1) return 1;
    if (dp[i] != -1) return dp[i];
    return dp[i] = f(i - 1) + f(i - 2);
}
```

**Tabulated (bottom-up):**
```cpp
int f(int n) {
    vector<int> dp(n + 1);
    dp[0] = 1;
    if (n >= 1) dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2]; // same recurrence, computed in dependency order
    }
    return dp[n];
}
```

Notice the recurrence itself — `dp[i] = dp[i-1] + dp[i-2]` — is **identical** in both versions. Only the *direction* of computation changed.

---

## 📉 Tabulation → Space Optimization

Once tabulation works, check: **does every row/index actually need to stay in memory, or do I only ever look back a fixed number of steps?**

### Example

```cpp
dp[i] = dp[i - 1] + dp[i - 2];
```

Only the **last two values** are ever needed to compute the next one — the rest of the array is dead weight.

**O(n) space:**
```cpp
vector<int> dp(n + 1);
dp[0] = 1; dp[1] = 1;
for (int i = 2; i <= n; i++) dp[i] = dp[i - 1] + dp[i - 2];
return dp[n];
```

**O(1) space:**
```cpp
int prev2 = 1, prev1 = 1;
for (int i = 2; i <= n; i++) {
    int curr = prev1 + prev2;
    prev2 = prev1;
    prev1 = curr;
}
return prev1;
```

### The General Rule
1. Identify how many previous states each `dp[i]` depends on (here: 2 — `i-1` and `i-2`).
2. Keep only that many rolling variables instead of the whole array.
3. For 2D DP (e.g., `dp[i][j]` depending only on row `i-1`), you often only need to keep **two rows** instead of the full grid, using the same rolling-variable idea one dimension up.

> **Space optimization is always the *last* step.** Optimize only after tabulation is verified correct — an optimized-but-wrong solution is far harder to debug than an unoptimized-but-correct one.

---

## 🗂️ Major DP Categories

| Category | Typical State | Main Pattern | Examples |
|---|---|---|---|
| 1D DP | `f(i)` | Linear dependency | Fibonacci, Climbing Stairs |
| 2D DP | `f(i, j)` | Two-sequence / two-index comparison | LCS, Edit Distance |
| Take/Not Take | `f(i, ...)` | Binary decision per element | House Robber, Target Sum |
| 0/1 Knapsack | `f(i, capacity)` | Take once or skip, capacity shrinks | 0/1 Knapsack, Subset Sum |
| Unbounded Knapsack | `f(i, capacity)` | Take unlimited times | Coin Change, Rod Cutting |
| Grid DP | `f(row, col)` | Movement across a 2D grid | Unique Paths, Minimum Path Sum |
| Subsequence DP | `f(i, j)` | Compare/build subsequences | LCS, LIS, Distinct Subsequences |
| String DP | `f(i, j)` | Character-by-character comparison | Edit Distance, Word Break |
| Partition DP | `f(l, r)` with split `k` | Try every partition point | Palindrome Partitioning, MCM |
| Interval DP | `f(l, r)` | Range shrinks inward | Burst Balloons, Optimal BST |
| Tree DP | `f(node, state)` | Postorder aggregation | Tree diameter, House Robber III |
| DAG DP | `f(node)` | Longest/shortest path on a DAG | Longest Increasing Path |
| Bitmask DP | `f(mask, i)` | Subset represented as bits | TSP, Assignment problems |
| Digit DP | `f(pos, tight, ...)` | Digit-by-digit number construction | Count numbers with a property |
| State Compression | Rolling variables/rows | Reduce dimensionality of storage | Any space-optimized DP |

---

## 🗺️ DP Learning Roadmap

### Phase 1 — Recursion
**Topics**: Base cases · Recursive state · Choices · Recursion tree · Complexity
**Problems**: Factorial · Fibonacci · Climbing Stairs · Subsequences · Subsets · Permutations

### Phase 2 — 1D DP
**Problems**: Fibonacci · Climbing Stairs · Frog Jump · Frog Jump with K Jumps · Maximum Sum of Non-Adjacent Elements · House Robber · Min Cost Climbing Stairs

### Phase 3 — Take / Not Take
**Problems**: House Robber · Subset Sum · Partition Equal Subset Sum · 0/1 Knapsack · Target Sum

### Phase 4 — 2D DP
**Problems**: Unique Paths · LCS · Edit Distance · Distinct Subsequences

### Phase 5 — Knapsack
**Problems**: 0/1 Knapsack · Unbounded Knapsack · Coin Change · Coin Change II · Rod Cutting · Target Sum

### Phase 6 — Subsequence DP
**Problems**: LCS · LIS · Longest Palindromic Subsequence · Distinct Subsequences · Edit Distance

### Phase 7 — Partition / Interval DP
**Problems**: Matrix Chain Multiplication · Palindrome Partitioning · Burst Balloons · Minimum Cost to Cut a Stick · Boolean Parenthesization

### Phase 8 — Tree DP
**Topics**: Choose / Don't Choose · Parent-child state · Postorder DP · Rerooting

### Phase 9 — Advanced DP
**Topics**: Bitmask DP · Digit DP · DP on DAGs · DP + Binary Search · DP + Monotonic Queue · DP Optimization · State Compression · Rerooting DP

---

## 🔍 Pattern Recognition Cheat Sheet

| Problem wording | Likely DP pattern |
|---|---|
| "Choose or skip" | Take / Not Take |
| "How many ways?" | Counting DP |
| "Minimum cost" | Min DP |
| "Maximum value" | Max DP |
| "Is it possible?" | Boolean DP |
| "Move through matrix" | Grid DP |
| "Two strings" | 2D / String DP |
| "Subsequence" | Subsequence DP |
| "Split into parts" | Partition DP |
| "[l, r]" range | Interval DP |
| "Items + capacity" | Knapsack |
| "Tree + decisions" | Tree DP |
| "Digit restrictions" | Digit DP |

---

## 🧭 Universal DP Template

Use this checklist for **every new DP problem**, in order, without skipping steps.

### Step 1 — What does `f(...)` mean?
Write a one-sentence definition of what the function returns.

### Step 2 — What is the state?
List every variable that changes as the recursion proceeds.

### Step 3 — What are the choices?
At this state, what decisions can be made?

### Step 4 — What happens after each choice?
How does the state change once a choice is made?

### Step 5 — What is the base case?
What is the smallest input that can be answered directly?

### Step 6 — Is it min / max / count / boolean?
Classify using the cheat sheet above.

### Step 7 — Write recursion
Implement the pure recursive version. Verify correctness on small inputs before optimizing anything.

### Step 8 — Check repeated states
Trace the recursion tree — are the same `(state)` calls appearing more than once?

### Step 9 — Memoize
Add a cache check + store, keeping the recursive logic unchanged.

### Step 10 — Tabulate
Rebuild bottom-up, filling states in dependency order.

### Step 11 — Optimize space
Reduce storage to only what's actually needed for future computations.

---

## 💻 Reusable C++ Templates

### 1D DP
```cpp
int dp[MAXN];
int f(int i) {
    if (/* base case */) return baseValue;
    if (dp[i] != -1) return dp[i];
    return dp[i] = /* recurrence using f(i-1), f(i-2), ... */;
}
```

### 2D DP
```cpp
int dp[MAXN][MAXM];
int f(int i, int j) {
    if (/* base case */) return baseValue;
    if (dp[i][j] != -1) return dp[i][j];
    return dp[i][j] = /* recurrence */;
}
```

### Take / Not Take
```cpp
int f(int i, int target) {
    if (i == 0) return /* base case handling */;
    int notTake = f(i - 1, target);
    int take = (target >= arr[i]) ? val[i] + f(i - 1, target - arr[i]) : INT_MIN;
    return max(take, notTake);
}
```

### Min DP
```cpp
int f(int i) {
    if (i == 0) return 0;
    int best = INT_MAX;
    for (/* each choice */) best = min(best, cost + f(nextState));
    return best;
}
```

### Max DP
```cpp
int f(int i) {
    if (i == 0) return 0;
    int best = INT_MIN;
    for (/* each choice */) best = max(best, value + f(nextState));
    return best;
}
```

### Counting DP
```cpp
const int MOD = 1e9 + 7;
int f(int i) {
    if (/* base case */) return 1;
    int ways = 0;
    for (/* each choice */) ways = (ways + f(nextState)) % MOD;
    return ways;
}
```

### Boolean DP
```cpp
bool f(int i, int target) {
    if (target == 0) return true;
    if (i == 0 || target < 0) return false;
    return f(i - 1, target) || f(i - 1, target - arr[i]);
}
```

### Grid DP
```cpp
int f(int row, int col) {
    if (row == 0 && col == 0) return grid[0][0];
    if (row < 0 || col < 0) return INT_MAX; // or appropriate sentinel
    int up = f(row - 1, col);
    int left = f(row, col - 1);
    return grid[row][col] + min(up, left);
}
```

### Partition DP
```cpp
int f(int l, int r) {
    if (l >= r) return 0;
    int best = INT_MAX;
    for (int k = l; k < r; k++) {
        int cost = f(l, k) + f(k + 1, r) + mergeCost(l, k, r);
        best = min(best, cost);
    }
    return best;
}
```

### Interval DP
```cpp
int f(int l, int r) {
    if (l > r) return 0;
    if (dp[l][r] != -1) return dp[l][r];
    int best = INT_MAX;
    for (int k = l; k <= r; k++) {
        int cost = f(l, k - 1) + f(k + 1, r) + intervalCost(l, k, r);
        best = min(best, cost);
    }
    return dp[l][r] = best;
}
```

---

## 📋 Problem Tracker

| # | Problem | Category | Pattern | Difficulty | Recursion | Memoization | Tabulation | Space Optimized | Notes |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Climbing Stairs | 1D DP | Counting | Easy | ⬜ | ⬜ | ⬜ | ⬜ | |
| 2 | Frog Jump | 1D DP | Min DP | Easy | ⬜ | ⬜ | ⬜ | ⬜ | |
| 3 | House Robber | Take/Not Take | Max DP | Medium | ⬜ | ⬜ | ⬜ | ⬜ | |
| 4 | 0/1 Knapsack | Knapsack | Take/Not Take | Medium | ⬜ | ⬜ | ⬜ | ⬜ | |
| 5 | Unique Paths | Grid DP | Counting | Medium | ⬜ | ⬜ | ⬜ | ⬜ | |
| 6 | LCS | Subsequence DP | 2D DP | Medium | ⬜ | ⬜ | ⬜ | ⬜ | |
| 7 | Edit Distance | String DP | 2D DP | Hard | ⬜ | ⬜ | ⬜ | ⬜ | |
| 8 | Matrix Chain Multiplication | Interval DP | Partition | Hard | ⬜ | ⬜ | ⬜ | ⬜ | |

> Add new rows as you solve more problems — this table is meant to grow with the repository.

---

## ✅ Pattern Mastery Tracker

For each category, all of the following should be true before you consider it "mastered":

- [ ] I can define the state
- [ ] I can explain what `f(state)` means
- [ ] I can identify the choices
- [ ] I can write the recurrence
- [ ] I can identify the base case
- [ ] I can write recursion
- [ ] I can memoize
- [ ] I can tabulate
- [ ] I can optimize space
- [ ] I can recognize the pattern in a new problem

**Recursion** · **1D DP** · **Take/Not Take** · **Min/Max DP** · **Counting DP** · **Boolean DP** · **Grid DP** · **Knapsack** · **Subsequence DP** · **Partition DP** · **Interval DP** · **Tree DP** · **Advanced DP**

> Copy the checklist above under each category heading as you work through the roadmap.

---

## ⚠️ Common Mistakes

- **Not defining the state** — jumping into code before knowing what `f(...)` means.
- **Wrong or incomplete state** — missing a variable that two different situations actually need to distinguish them.
- **Wrong base case** — off-by-one or logically incorrect stopping condition.
- **Missing a choice** — forgetting an option in the decision tree (e.g., forgetting "skip" and only ever "taking").
- **Mixing current and next state** — using `i` where `i-1` (or vice versa) was intended.
- **Confusing min/max/count/boolean** — applying `max()` to a counting problem, or `+` to an optimization problem.
- **Jumping directly to tabulation** — skipping recursion means you never validate the recurrence itself.
- **Incorrect iteration order** — filling `dp[i]` before the states it depends on are ready.
- **Incorrect memoization dimensions** — cache array doesn't match the actual number of state variables.
- **INT_MAX overflow** — adding to `INT_MAX` before comparing, causing silent wraparound.
- **Off-by-one errors** — especially common in 1-indexed vs 0-indexed array problems.
- **Optimizing too early** — attempting space optimization before tabulation is even verified correct.
- **Memorizing solutions instead of understanding patterns** — the single biggest reason DP feels "impossible to learn."

---

## ❓ Questions to Ask When Stuck

1. What does `f(...)` mean?
2. What information changes?
3. What is my state?
4. What choices do I have?
5. What happens after each choice?
6. What is the base case?
7. Am I minimizing, maximizing, counting, or checking possibility?
8. Can the same state appear again?
9. What parameters uniquely identify a state?
10. What does the current state depend on?
11. What should my iteration order be?
12. Can I reduce the space?

---

## 🏆 Golden Rules

1. Don't memorize DP code.
2. Define the state before coding.
3. Always know what `f(state)` means.
4. Find the choices before writing transitions.
5. Write recursion first when learning.
6. Memoization is recursion + storage.
7. Tabulation is the same recurrence in reverse dependency order.
8. Space optimization comes last.
9. If you cannot explain the recurrence, you don't understand the DP yet.
10. Pattern recognition is more valuable than memorizing individual problems.

---

## 🧭 Final Mental Model

```
                 NEW DP PROBLEM
                       │
                       ▼
                What is the state?
                       │
                       ▼
                 What are my choices?
                       │
                       ▼
                Write the recursion
                       │
                       ▼
                 Find base cases
                       │
                       ▼
             Do states repeat?
                  /         \
                YES          NO
                 │            │
                 ▼            ▼
             Memoize      Recursion
                 │
                 ▼
             Tabulate
                 │
                 ▼
         Optimize Space
                 │
                 ▼
          MASTER THE PATTERN
```

> **"The goal is not to remember the solution.**
> **The goal is to remember how to derive the solution."**

*End of Reference Sheet*
