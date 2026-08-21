# CP Reference Sheet

> **VS Code Preview:** `Cmd+Shift+V` (Mac) or `Ctrl+Shift+V` (Windows/Linux)  
> **Jump sections:** `Cmd+Shift+O` → Outline panel

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
| 7 | [Vectors](#7-vectors) | 17 | [Prefix Sum & MEX & Gap](#17-prefix-sum--mex--gap-method) |
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
- **Vectors/deques:** contiguous memory — erase shifts all elements after it left, so iterators at/after erased position become **invalid**
- **Maps/lists:** node-based — each element is its own heap allocation, so erasing one node does **not** move others → other iterators stay valid

```cpp
// Round-off: multiply to shift decimal right, round, shift back
x = round(x * 1e5) / 1e5;
```

---

## 2. Fast I/O & Includes

```cpp
// sync_with_stdio(false) : decouple C and C++ I/O buffers (huge speed gain)
// cin.tie(NULL)          : stop cout from flushing before every cin
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);
}
```

---

## 3. Overflow & Modulo

> ⚠️ **Overflow happens BEFORE modulo.** Fix the type FIRST, then apply %.

```cpp
// int * int can exceed 2*10^9 silently — the damage is done before % runs
long long x = a * b;       // ❌ a*b computed as int, already overflowed
(long long)(a * b);        // ❌ cast happens after the overflow
1LL * a * b;               // ✅ 1LL forces the whole chain into long long
```

### Mod Operations

```cpp
const int MOD = 1e9 + 7;

// Addition: both terms can be up to MOD-1, sum fits in int after %
(a + b) % MOD == ((a % MOD) + (b % MOD)) % MOD;

// Subtraction: result can go negative, +MOD pulls it back into [0, MOD)
(a - b) % MOD == ((a % MOD) - (b % MOD) + MOD) % MOD;

// Multiplication: product of two ~10^9 numbers needs long long
(a * b) % MOD == (1LL * a * b) % MOD;

// Division: no direct modular division — multiply by modular inverse instead
(a / b) % MOD == (1LL * a * modInverse(b)) % MOD;
```

### ODD / EVEN

```cpp
// Last bit of a number is 1 if odd, 0 if even
int result = (n & 1) ? 1 : 0;
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
// Distance from 'a' gives 0-indexed position, add 'A' to shift to uppercase
'c' - 'a' = 2  →  2 + 'A' = 'C'

// Lexicographic: compare left to right, first mismatch decides winner
// if all chars match up to shorter string's length → shorter is smaller
```

---

## 5. STL Algorithms

```cpp
#include <algorithm>
```

### Search

```cpp
// find: linear scan, returns iterator to first match or end
find(begin, end, x);

// binary_search: cuts search space in half each step — must be sorted
binary_search(begin, end, x);  // bool

// lower_bound: first position where x can be inserted without breaking sort (>=x)
// upper_bound: first position after all x's (>x)
// both return end() if nothing qualifies
lower_bound(begin, end, x);
upper_bound(begin, end, x);
```

### Count / Sum / Min / Max

```cpp
count(begin, end, x);          // walks the range and tallies matches — O(n)
accumulate(begin, end, 0);     // running total from left to right — O(n)
min_element(begin, end);       // single pass tracking the running minimum
max_element(begin, end);       // single pass tracking the running maximum
```

### Sort / Order

```cpp
sort(begin, end);              // introsort (quicksort + heapsort fallback) O(n log n)
reverse(begin, end);           // swap from both ends walking inward — O(n)
is_sorted(begin, end);         // checks adjacent pairs — O(n)
next_permutation(begin, end);  // rearranges to next lexicographic order — O(n)
```

### Remove & Erase

```cpp
// remove() logic: walk the array, copy non-matching elements to the front
// returns iterator to new logical end — SIZE IS UNCHANGED, tail is garbage
// erase() then actually shrinks the container

// Erase-remove idiom for vectors:
v.erase(remove(v.begin(), v.end(), x), v.end());

// Same for strings:
s.erase(remove(s.begin(), s.end(), 'a'), s.end());
```

### Permutations

```cpp
// next_permutation rearranges to the next lexicographic order
// stops and returns false when already at the largest permutation
// so sort first to start from the smallest permutation
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
s.size();    s.length();
s[i];        s.at(i);      // at() throws if out of range, [] doesn't
s.pop_back();

// find: scans left to right, returns index of first match
s.find(x);                 // returns string::npos if not found

// substr: copies len characters starting at pos
s.substr(pos, len);

// erase by index: removes count chars starting at position
s.erase(pos, count);

// erase by iterator: removes [it1, it2) range
s.erase(it1, it2);
```

### Char Type Functions

```cpp
// these avoid hardcoding ASCII math
isalnum(c);   // letter or digit?
isdigit(c);   // '0'–'9'?
tolower(c);   // uppercase → lowercase (adds 32 to ASCII value)
toupper(c);   // lowercase → uppercase (subtracts 32)
```

### Rotate / All Rotations

```cpp
// rotate(new_begin): elements from new_begin move to front, rest follow
rotate(s.begin(), s.begin() + k, s.end());

// All rotations trick: s+s contains every rotation as a contiguous substring
// e.g. "abc" + "abc" = "abcabc" → "bca" and "cab" are both substrings
string doubled = s + s;
```

---

## 7. Vectors

```cpp
// emplace_back constructs in-place — avoids copy, faster than push_back
vector<int> v;
vector<int> v = {a, b, c};   // ✅ initializer list
// vector<int> v[3] = {a, b, c};  ❌ this declares an array of 3 vectors

v.push_back(x);     v.emplace_back(x);
v.pop_back();       v.clear();
v.begin();          v.end();
v.back();           v.empty();
v.size();           v.swap(v2);

// erase single: shifts all elements after it one left — O(n)
v.erase(it);

// erase range [begin, end): removes the entire range — O(n)
v.erase(begin, end);

// insert at position: shifts everything right — O(n)
v.insert(v.begin() + index, value);
```

### Iterator Invalidation After Erase

```cpp
// erase() returns iterator to the element that replaced the erased one
// use this to safely continue iterating
it = v.erase(it);   // ✅ it now points to next valid element

// DO NOT use an old iterator after erase — memory has shifted
auto it2 = v.begin() + 1;
v.erase(it2);
cout << *it2;        // ❌ INVALID — element gone, memory shifted
```

---

## 8. Stack / Queue / Priority Queue

### Stack — LIFO

> ⚠️ Stack is **NOT** iterable — no range-based for loop!

```cpp
// LIFO: last pushed is first popped
// top() + pop() without empty check = undefined behavior
stack<int> s;
s.push(a);    s.emplace(a);   // emplace constructs in-place, NOT emplace_back
s.top();                      // peek without removing
s.size();
s.pop();      // ALWAYS check !s.empty() before pop
s.empty();
```

### Queue — FIFO

```cpp
// FIFO: first pushed is first popped
// front() = oldest element, back() = newest
queue<int> q;
q.push(1);    q.emplace(a);
q.front();    q.back();
q.pop();      // removes from front — check !q.empty() first
q.empty();
```

### Priority Queue

```cpp
// internally a max-heap: largest element always at top
// push/pop maintain heap property in O(log n)
priority_queue<int> pq;                                 // max at top (default)
priority_queue<int, vector<int>, greater<int>> pq_min;  // min at top

pq.push(a);    pq.emplace(a);
pq.top();      // peek the max (or min)
pq.pop();      // remove the top
```

---

## 9. Set / Map / Multiset

### Set

```cpp
// Internally a balanced BST (red-black tree)
// Guarantees sorted order + uniqueness + O(log n) all operations
set<int> st;

// unordered_set uses a hash table — O(1) avg but no order
unordered_set<int> us;

st.insert(1);    st.emplace(1);
st.size();
st.begin();      st.end();

// find returns iterator to element, or st.end() if not found
st.find(x);

// erase by iterator: removes that exact node
st.erase(it);

// erase range: removes [find(2), find(4)) — note: half-open interval
st.erase(st.find(2), st.find(4));

// count returns 0 or 1 for set (use as existence check)
st.count(x);

// lower_bound/upper_bound: like binary search but built into the BST
st.lower_bound(x);    // iterator to first element >= x
st.upper_bound(x);    // iterator to first element > x
```

### Multiset

```cpp
// like set but allows duplicate elements
// find always returns the FIRST occurrence
// erase by value removes ALL copies — erase by iterator removes ONE
multiset<int> mst;

mst.insert(1);
mst.count(x);                    // total occurrences of x
auto it = mst.find(2);           // iterator to first 2
mst.erase(mst.find(x));         // ✅ remove exactly ONE copy of x
mst.erase(x);                    // ⚠️  removes ALL copies of x
mst.upper_bound(x);
mst.lower_bound(x);
```

### Map

```cpp
// keys stored in sorted order (BST internally)
// inserting a duplicate key overwrites the value — old value is lost
map<int, int> mp;
map<int, int> mp = {{1,10}, {2,20}};

// operator[] creates the key with default value 0 if it doesn't exist!
// use find() when you don't want accidental insertion
mp[key];                  // O(log n) — creates if missing
mp.insert({k, v});        // O(log n) — does NOT overwrite existing key
mp.find(x);               // iterator or mp.end()
mp.count(key);            // 0 or 1 — safe existence check
mp.erase(key or it);
mp.clear();
mp.lower_bound(x);    mp.upper_bound(x);

// iterate as key-value pairs
for (auto [k, v] : mp) { ... }         // structured binding (C++17)
for (auto& p : mp) { p.first; p.second; }
```

### Unordered Map

```cpp
// hash table: no ordering, O(1) avg for all ops
// worst case O(n) if many hash collisions (rare in practice)
unordered_map<int, int> mp;

mp[key];               // creates with 0 if missing — same trap as map
mp.find(x);            // O(1) avg
mp.count(key);         // O(1) avg
mp.erase(key or it);
mp.clear();
```

### Check Unique Values in Map

```cpp
// walk all values, insert into a set — if already seen, not unique
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
// Heap property: parent <= both children (min-heap)
// Stored as array: parent of i = (i-1)/2, children = 2i+1 and 2i+2
// insert: add at end, then bubble UP (swap with parent while smaller)
// extract: move last to root, then bubble DOWN (swap with smaller child)

class MinHeap {
    vector<int> h;
public:
    // add at end, bubble up to restore heap property
    void insert(int x) {
        h.push_back(x);
        int i = h.size() - 1;
        while (i > 0 && h[(i-1)/2] > h[i]) {
            swap(h[i], h[(i-1)/2]);
            i = (i-1)/2;
        }
    }

    int getMin() { return h[0]; }

    // push the smaller child up to restore heap property downward
    void heapify(int i) {
        int s = i, l = 2*i+1, r = 2*i+2;
        if (l < h.size() && h[l] < h[s]) s = l;
        if (r < h.size() && h[r] < h[s]) s = r;
        if (s != i) { swap(h[i], h[s]); heapify(s); }
    }

    // replace root with last element, shrink array, heapify down
    int extractMin() {
        int root = h[0];
        h[0] = h.back();
        h.pop_back();
        heapify(0);
        return root;
    }

    // set new smaller value, then bubble up like insert
    void decreaseKey(int i, int val) {
        h[i] = val;
        while (i > 0 && h[(i-1)/2] > h[i]) {
            swap(h[i], h[(i-1)/2]);
            i = (i-1)/2;
        }
    }

    // trick: decrease to -inf so it bubbles to top, then extract
    void deleteKey(int i) {
        decreaseKey(i, INT_MIN);
        extractMin();
    }
};
```

---

## 11. Monotonic Stack

> Used for: **Next Greater / Smaller Element**.  
> Key idea: maintain a stack that is always increasing (or decreasing). When the current element breaks the order, the stack top has found its answer.

```cpp
// Increasing stack: pop anything GREATER than current (they'll never be NGE)
// → stack always has potential "next greater" candidates in order
while (!st.empty() && st.top() > a[i])
    st.pop();

// Decreasing stack: pop anything SMALLER than current
while (!st.empty() && st.top() < a[i])
    st.pop();
```

### Template

```cpp
// push index when you need to compute distances or store positions
for (int i = 0; i < n; i++) {
    // current element breaks monotonic property → element at top found its answer
    while (!st.empty() && condition_on_stack_top)
        st.pop();
    st.push(a[i]);   // or push index i
}
// Time: O(n) — each element pushed and popped at most once
```

---

## 12. Two Pointers

> Pointers are **ROLES**, not just indices. Fix each pointer's job and never change it.

```cpp
// works when moving one pointer always makes the condition better or worse
// if sum too big → shrink from right, too small → grow from left
while (l < r) {
    if (condition) l++;
    else r--;
}
```

### Three Pointers — Dutch Flag

```
Invariant at all times:
  [0 → low-1]   = zeros
  [low → mid-1] = ones
  [mid → high]  = unsorted
  [high+1 → n]  = twos
```

### Rotate Array by K

```cpp
// Observation: reversing reverses the order; two targeted reverses = rotation
// reverse all → reverse first k → reverse rest k+1..n
reverse(nums.begin(), nums.end());          // step 1
reverse(nums.begin(), nums.begin() + k);   // step 2
reverse(nums.begin() + k, nums.end());     // step 3
```

---

## 13. Sliding Window

```cpp
// window [l, r] expands by moving r
// when window becomes invalid, shrink from left until valid again
// r never goes back → each element enters and leaves the window at most once → O(n)

int l = 0;
while (r < n) {
    // add element at r to window state

    while (/* window condition is broken */) {
        // remove element at l from window state
        l++;
    }

    r++;
}
```

---

## 14. Binary Search

> Binary search = **ELIMINATION**. Each step cuts the search space in half.  
> Not just for finding values — use it any time you can decide "go left or go right".

```cpp
// invariant: answer always lies in [l, r]
// mid = l + (r-l)/2 avoids integer overflow vs (l+r)/2
while (l <= r) {
    int mid = l + (r - l) / 2;

    if (/* mid satisfies condition — maybe answer, try smaller */)
        r = mid - 1;
    else
        l = mid + 1;
}
```

### Search in Rotated Array

```cpp
// One half is always sorted — find which half, then check if target is in it
// If it is → search that half. If not → search the other half.
// Edge case: duplicates at both ends can hide which side is sorted → shrink both

if (nums[low] == nums[mid] && nums[high] == nums[mid]) {
    low++; high--;   // can't determine sorted half, shrink both ends
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

> Find the majority element (appears > n/2 times) in **O(n) time, O(1) space**.  
> Intuition: majority element = +1, every other element = -1. If it's truly majority, it survives all cancellations.

```cpp
int el = 0, cnt = 0;
for (int i : nums) {
    if (cnt == 0) { el = i; cnt = 1; }   // no current candidate, adopt this one
    else if (i == el) cnt++;              // same as candidate, reinforce it
    else cnt--;                           // different, cancel one out
}
// el is the survivor — verify in a second pass if majority isn't guaranteed
```

---

## 16. Kadane's Algorithm

> Max sum subarray.  
> Intuition: a negative running sum makes the total worse than just starting fresh from the next element. Reset to 0 whenever sum goes negative.

```cpp
int curr_sum = nums[0], max_sum = nums[0];
for (int i = 1; i < n; i++) {
    // either extend current subarray, or start a new one from here
    curr_sum = max(nums[i], curr_sum + nums[i]);
    max_sum  = max(max_sum, curr_sum);
}
```

### With Index Tracking

```cpp
int sum = 0, maxi = INT_MIN, start = 0, end = 0, s = 0;
for (int i = 0; i < n; i++) {
    if (sum == 0) s = i;           // potential new start
    sum += nums[i];
    if (sum > maxi) { maxi = sum; start = s; end = i; }
    if (sum < 0) sum = 0;          // negative sum → discard, reset
}
```

---

## 17. Prefix Sum / MEX / Gap Method

### Prefix Sum

```cpp
// pre[i] = sum of arr[0..i]
// range sum [l, r] = pre[r] - pre[l-1]  (avoids re-summing every query)
vector<int> pre(n);
pre[0] = arr[0];
for (int i = 1; i < n; i++)
    pre[i] = pre[i-1] + arr[i];

int rangeSum = pre[r] - (l > 0 ? pre[l-1] : 0);
```

### Prefix GCD

```cpp
// gcd shrinks or stays the same as more elements are included
// prefix[i] = gcd of all elements from 0 to i
prefix[0] = arr[0];
for (int i = 1; i < n; i++)
    prefix[i] = gcd(prefix[i-1], arr[i]);
```

### MEX (Minimum Excludant)

```cpp
// smallest non-negative integer NOT in the array
// dump into hash set for O(1) lookup, then scan 0, 1, 2... until missing
int mex(vector<int>& a) {
    unordered_set<int> s(a.begin(), a.end());
    int x = 0;
    while (s.count(x)) x++;
    return x;
}
```

### Gap Method — Merge Two Sorted Arrays In-Place

```cpp
// Treat both arrays as one virtual array of size m+n
// Start with gap = ceil((m+n)/2), compare & swap elements that far apart
// Halve gap each round — by the time gap=1, array is sorted (like shell sort)

int len = m + n;
for (int gap = (len+1)/2; gap > 0; gap = (gap == 1) ? 0 : (gap+1)/2) {
    for (int l = 0, r = gap; r < len; l++, r++) {
        // figure out which actual array each index belongs to, then swap if needed
        if      (l < n && r >= n) swap_if_greater(a[l],   b[r-n]);
        else if (l >= n)          swap_if_greater(b[l-n],  b[r-n]);
        else                      swap_if_greater(a[l],   a[r]);
    }
}
```

### Generate All Subsets

```cpp
// At each index, make a binary choice: include this element or skip it
// Recursion tree has 2^n leaves, each leaf = one subset
void findSubset(int ind, vector<int>& nums,
                vector<int> ds, vector<vector<int>>& ans) {
    if (ind == nums.size()) { ans.push_back(ds); return; }

    ds.push_back(nums[ind]);
    findSubset(ind+1, nums, ds, ans);   // take nums[ind]
    ds.pop_back();
    findSubset(ind+1, nums, ds, ans);   // skip nums[ind]
}
// Call: findSubset(0, nums, {}, ans);
```

---

## 18. Math & Number Theory

### GCD & LCM

```cpp
// Euclidean: gcd(a,b) = gcd(b, a%b) — remainder shrinks until it hits 0
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
// LCM: divide first to avoid overflow before multiplying
int lcm(int a, int b) { return a / gcd(a, b) * b; }
```

### Count Divisors — sqrt approach

```cpp
// Divisors come in pairs (i, n/i) — only need to check up to sqrt(n)
// If i == n/i (perfect square), count once; otherwise count both
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

### Modular Inverse — Fermat's Little Theorem

```cpp
// (a * x) % m = 1  →  x is the modular inverse of a
// By Fermat: x = a^(m-2) % m  (only works when m is prime and gcd(a,m)=1)
// Use fast exponentiation to compute a^(m-2) % m
```

### Fast Exponentiation

```cpp
// Binary exponentiation: represent exponent in binary
// if bit is 1 → multiply result by current base
// always square the base and shift exponent right
// O(log n) multiplications instead of O(n)
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
// Start from 2; every multiple of a prime is composite → mark it
// Start marking from i*i (smaller multiples already marked by earlier primes)
// O(n log log n) time
vector<int> sieve(int n) {
    vector<int> nums(n+1, 1);
    nums[0] = nums[1] = 0;
    for (int i = 2; i*i <= n; i++) {
        if (nums[i])
            for (int j = i*i; j <= n; j += i)
                nums[j] = 0;
    }
    return nums;  // nums[i] == 1  →  i is prime
}
```

### nCr

```cpp
// nCr = n! / (r! * (n-r)!)
// Compute iteratively to avoid huge factorials
// Symmetry: nCr == nC(n-r) → pick whichever r is smaller to reduce loops
long long nCr(int n, int r) {
    if (r > n) return 0;
    if (r == 0 || r == n) return 1;
    if (r > n - r) r = n - r;
    long long ans = 1;
    for (int i = 1; i <= r; i++)
        ans = ans * (n - i + 1) / i;
    return ans;
}
```

### Precompute Factorials

```cpp
// When nCr needed many times, precompute fact[] and use modular inverse
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
1 + (int)log10(x);   // number of digits: log10 gives position of leading digit
(int)log2(n);        // position of highest set bit = number of bits - 1

// XOR properties:
// a ^ a = 0  (same number cancels)
// a ^ 0 = a  (XOR with 0 is identity)
// → XOR all elements: duplicates cancel, unique element survives

// pow() uses floating point → NOT for large integers, use fastPow() instead
```

---

## 19. Recursion & Backtracking

> **DRAW THE RECURSION TREE — it has all the hints.**

### Rules

```
- index increasing toward n  →  base case is (ind == n)
- index decreasing toward 0  →  base case is (ind == 0)
- function must be structurally consistent: f(n-1) should mirror what f(n) does
- you can replace loops/two-pointers with recursion (palindrome, reverse)
```

### Take or Not Take (Subsets)

```cpp
// at each position make a binary decision: include or skip
// two recursive branches → 2^n total subsets
ds.push_back(arr[i]);
f(index + 1, ds);     // branch 1: take
ds.pop_back();
f(index + 1, ds);     // branch 2: skip
```

### Return True on First Valid Subsequence

```cpp
// short-circuit: if one branch found a valid answer, don't explore the other
ds.push_back(arr[i]);
if (f(index+1, ds)) return true;    // found it in "take" branch
ds.pop_back();
if (f(index+1, ds)) return true;    // found it in "skip" branch
return false;
```

### Reverse Array Recursively

```cpp
// swap outermost pair, then recurse inward
// base: when begin reaches the middle, all pairs have been swapped
void reverseArr(int begin, vector<int>& arr) {
    int n = arr.size();
    if (begin >= n/2) return;
    swap(arr[begin], arr[n-begin-1]);
    reverseArr(begin + 1, arr);
}
// Call: reverseArr(0, arr);
```

### Palindrome Check

```cpp
// check outermost pair, recurse inward — mirrors reverseArr logic
bool palindrome(int i, vector<int>& arr) {
    int n = arr.size();
    if (i >= n/2) return true;
    if (arr[i] != arr[n-i-1]) return false;
    return palindrome(i + 1, arr);
}
```

### Permutations — Frequency Map

```cpp
// at each level, try placing each unused element at current position
// mark used before recursing, unmark after (classic do-explore-undo)
void permutate(vector<int>& nums, vector<vector<int>>& ans,
               vector<int> ds, unordered_map<int,int> used) {
    if (ds.size() == nums.size()) { ans.push_back(ds); return; }
    for (int i = 0; i < nums.size(); i++) {
        if (!used[nums[i]]) {
            ds.push_back(nums[i]);        // do
            used[nums[i]] = 1;
            permutate(nums, ans, ds, used);  // explore
            ds.pop_back();                // undo
            used[nums[i]] = 0;
        }
    }
}
```

### Permutations — Swap-Based (O(1) extra space)

```cpp
// fix one element at position ind by swapping it there
// recurse for ind+1, then swap back to restore original array
void permutate2(int ind, vector<vector<int>>& ans, vector<int>& nums) {
    if (ind == nums.size()-1) { ans.push_back(nums); return; }
    for (int i = ind; i < nums.size(); i++) {
        swap(nums[ind], nums[i]);           // place nums[i] at position ind
        permutate2(ind+1, ans, nums);
        swap(nums[ind], nums[i]);           // restore
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

```
Depth = edges from root to node  (root depth = 0)
Height = edges from node to its deepest leaf
In linked lists: ALWAYS use a dummy node to handle null head edge cases cleanly
```

### Inorder Traversal — Left → Root → Right

```cpp
// recursive: go as far left as possible, process, then go right
void inorder(TreeNode* root, vector<int>& store) {
    if (!root) return;
    inorder(root->left, store);
    store.push_back(root->val);
    inorder(root->right, store);
}
```

### Level Order Traversal (BFS)

```cpp
// use a queue: process one level at a time
// snapshot the queue size at start of each level — that's how many nodes to process
// pattern: pop node → record value → push its children
vector<vector<int>> levelOrder(TreeNode* root) {
    if (!root) return {};
    queue<TreeNode*> q;
    vector<vector<int>> res;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();              // nodes in current level
        vector<int> level;
        for (int i = 0; i < sz; i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left)  q.push(node->left);   // push children for next level
            if (node->right) q.push(node->right);
        }
        res.push_back(level);
    }
    return res;
}
```

### Height of Complete Binary Tree

```cpp
// in a complete binary tree, left spine goes to the last level
// walk left pointers counting steps to get height
int getLeftHeight(TreeNode* root) {
    int h = 0;
    while (root) { h++; root = root->left; }
    return h;
}
```

### Sum Root to Leaf

```cpp
// each level appends a digit: running value = value*10 + current node's digit
// same idea as building a number left to right
// pass down: sum = sum * 10 + root->val
```

### Palindrome Number

```cpp
// reverse only the second half of the number's digits
// compare with the first half — avoids string conversion
// negative numbers and trailing zeros (except 0 itself) can never be palindromes
bool isPalindrome(int x) {
    if (x < 0 || (x % 10 == 0 && x != 0)) return false;
    int reversed = 0;
    while (x > reversed) {            // stop when we've reversed half the digits
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }
    // even length: x == reversed   |   odd length: x == reversed/10 (drop middle)
    return x == reversed || x == reversed / 10;
}
```

---

*End of Reference Sheet*
