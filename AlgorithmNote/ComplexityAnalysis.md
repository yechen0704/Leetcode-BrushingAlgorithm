# Complexitry Analysis
## Look at input scale before writing code
Complexity is not just for theory, it literally tells you which approaches are possible  
- If N can be up to 10^6 then:
  - O(n^2) = 10^12 -> TLE
  - Optimize to O(NlogN) & O(N)
---
## Common coding mistakes that break complexity
1. Passing by value instead of reference
```cpp
void foo(vector<int> v); // pass by value COPY the entire vector
void foo(vector<int>& v); // pass by reference : NO copy
```
Understand your language's parameter passing and avoid unnecessary copies
2. Not understanding interface implementations
In java
- List is an interface; implementation include ArrayList and LinkedList
  - ArrayList.get(i) is O(1)
  - LinkedList.get(i) is O(N)
If a method accepts a List<?> and you call list.get(i) inside a loop assuming it's O(1), but the actual object is a LinkedList, your algorithm may degrade from O(N) to O(N^2)
---
## Big-O basics
### Keep only the fastest-growing term
- constants don't matter, slower terms don't matter
### Big-O is an upper bound
As long as you give an upper bound, it's correct Big-O, though we usually want a tight one  

---
## Non-Recursion Algorithm Analysis
1. Standard nested loops
2. But: nested loops aren't always O(N^2)
   - Example : two-pointer / sliding window
   ```
   int lo= 0, hi = n - 1;
   while (lo < hi) {
    if (sum < target) lo++;
     else hi--;
   }
   ```
     - lo only moves forward
     - hi only moves backward
     - Neither pointer ever moves back and forth repeatedly
     - Total move <= 2N
Amortized Analysis :
If a pointer never retreats, its total number of moves <= N -> that dimension contributes linear, not quadratic, time.

---
## Data structures & Amortized Analysis
For data structures that grow / shrink, single operations sometimes are expensive, but on average they are cheap
#### Example : Dynamic array / ArrayList
1. Most Appends: O(1)
2. When full resize :
   - Allocate new Array, copy N element -> O(N) for that one append
3. But over a dequence of N appends:
   - Each element is moved a constant number of times
   - Total work: O(N) -> average per sppend: amortized O(1)
#### Example : Monotonic queue
1. Worst case for a single push:O(N)
2. But over N operations, each element is
   - pushed once
   - popped at most once
3. Total work: O(N) -> amortized cost per operation O(1)
4. Same Idea applies to
   - Hash Table rehashing
   - Vector / ArrayList resizing
   - Some union-find operations

---
## Recursive Algorithms : Think in terms of Trees
Every recursive algorithm = a traversal of a recursion tree  
so:  
1. Time Complexity : number of nodes * work per node
2. Space Complexity : recursion Depth + extra storage
#### Example : Coin change (top-down DP)
Let N = amount, K = coin.length
- Upper bound on nodes : O(K^N)
- Work per node: loop over K coins : O(K)
So rough upper bound:
Time = O(K^N) * O(K)  
Space = recursion depth = O(N)  
#### Adding memorization (top-down dp)
1. Each state amount is computed at most once
2. Number of distinct subproblems = N + 1
3. Work per state remains O(K)

So:  
Time = state * work_per_state = O(N * K)  
Space = memo array O(N) + recursion depth O(N) = O(N)
#### Bottom-up DP(tabulation)
- DP array size : amount+1 -> space O(N), no recursion stack
- Outer loop over N, inner loop over K -> time O(K*N)

---
## Backtracking : permutations vs subsets
Let N = nums.length
#### Permutations 
1. Work per node : O(N) (loop + copy when leaf)
2. Recursion tree:
     - level 0 : P(N, 0) = 1
     - Level 1 : P(N, 1) = N
     - Level 2 : P(N, 2) = N*(N-1)
     - ...
     - Level N : P(N, N) = N!
  - Total Nodes rough= O(N*N!)

So:  
- Time = O(node * work_per_node) = O(N*N!) * O(N)
- Space = store all results O(N*N!) + recursion depth O(N) = O(N*N!)
#### Subsets (power set)
1. Each node : O(N) (copy path + loop)
2. Recursion tree
   - Each level decides "take / not take" -> total subsets = 2^n
   - Evert node = one subset, so nodes = 2^n
  
So :  
Time = O(2^N * N)   
Space = Store all subsets  O(N * 2^N) + recursion depth O(N) 

