# Leetcode 752 Open The Lock
You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.

The lock initially starts at '0000', a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

---
## Key Obeservation
Every time we only move one wheel by one slot.  
Heve 2 special slow 0->9, 9->0  
start from 0000  
iterate all next state of current state, reach deadend to finish iterate
```
deadend = ["1001"]   target = "3000"
                                      0000
                     /    /    /    /      \    \    \    \
                 1000   9000  0100  0900  0010  0090  0001  0009
            / / / / \ \ \ \
2000 0000 1100 1900 1010 1090 1001 1009
     ^duplicate               ^deadend
/
3000 <target
```
1. check 0000 inside deadend or not : valid start point
2. check cur state == target ? return step & end followed
3. each tree level have same step, so need sz to cauculate cur node numbers 
4. for loop all position, getUp and getDown (0,9 for special operation)
5. push inside queue

## code
```
class Solution {
    public int openLock(String[] deadends, String target) {
        String start = "0000";
        HashSet<String> set = new HashSet<>(Arrays.asList(deadends));
        Queue<String> queue = new LinkedList<>();
        if (set.contains(start) || set.contains(target)) return -1;
        queue.add(start);
        int step = 0;
        while (!queue.isEmpty()) {
            int sz = queue.size();
            for (int i = 0; i < sz; i++) {
                // cur state
                String cur = queue.poll();
                // check cur state 
                if (cur.equals(target)) return step;
                // for loop next state for 4 position
                for (int j = 0; j < 4; j++) {
                    // check deadends, skip
                    // otherwise queue add cur state
                    String upState = getUp(cur.toCharArray(), j);
                    if (!set.contains(upState)) {
                        set.add(upState);
                        queue.add(upState);
                    }
                    String downState = getDown(cur.toCharArray(), j);
                    if (!set.contains(downState)){
                        set.add(downState);  
                        queue.add(downState);
                    } 
                }
            }
            step++;
        }
        return -1;
    }

    private String getUp(char[] state, int p) {
        if (state[p] - '0' == 9){
            state[p] = '0';
            return new String(state);
        }
        state[p] = (char)(state[p] + 1);
        return new String(state);
    }
    private String getDown(char[] state, int p) {
        if (state[p] - '0' == 0){
            state[p] = '9';
            return new String(state);
        }
        state[p] = (char)(state[p] - 1);
        return new String(state);
    }
}
```
**Time Complexity:**
O(10^4 * 8) worst but actually won't reach this level
example : 0000 -> 5555 deadend=[]
Why not exponent ? use memo cutting edge, so same edge won't visit duplication
**Space Complexity:**
O(10^4)
