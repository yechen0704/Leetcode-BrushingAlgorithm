# Sliding Puzzle
On an 2 x 3 board, there are five tiles labeled from 1 to 5, and an empty square represented by 0. A move consists of choosing 0 and a 4-directionally adjacent number and swapping it.

The state of the board is solved if and only if the board is [[1,2,3],[4,5,0]].

Given the puzzle board board, return the least number of moves required so that the state of the board is solved. If it is impossible for the state of the board to be solved, return -1.

## Core Observation
Everytime actually move 0 in table  
Only when board looks like [1,2,3][4,5,0] 
```
                  412
                  503
           /       |      \
         412      402     412
         053      513     530
        / \      /  \      / \
      012 412  042  420   412  410
      453 503  513  513   503  532
       /                   ^dup => memo to avoid
    102
    453
    /
  102
  453
  /
120
453
/
123
450 <= target ......
```
1. check int[][] value equal ?  int[][] store by reference, cannot check hash to deduplicate. and If we deep copy original board it's poor performance
   - So we need a data structure to store board state and easy copy and compare => string
2. for each direction that 0 can move, wo iterate in it's layer
   - String index in array-based direction : {{1,3}, {0,2,4}, {1,5}, {0, 4}, {1,3,5}, {2,4}}
3. So use BFS and memo interate all 0's move and then check curBoard = target

## Code
```java
class Solution {
    public int slidingPuzzle(int[][] board) {
        String target = "123450";
        int[][] dirs = new int[][]{{1,3}, {0,2,4}, {1,5}, {0, 4}, {1,3,5}, {2,4}};
        HashSet<String> memo = new HashSet<>();
        int step = 0;

        StringBuilder start = new StringBuilder();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                start.append(board[i][j]);
            }
        }
        Queue<String> queue = new LinkedList<>();
        queue.offer(start.toString());
        memo.add(start.toString());
        while (!queue.isEmpty()) {
            int sz = queue.size();
            for(int i = 0; i < sz; i++) {
                // start from board 
                String cur = queue.poll();
                // check cur == target? return step & end search
                if (cur.equals(target)) return step;
                // for loop by direction, swap target char with current 0 position, push into queue
                int idx = 0;
                for (; cur.charAt(idx) != '0'; idx++);
                for (int adj : dirs[idx]) {
                    String new_board = swap(cur.toCharArray(), adj, idx);
                    if (!memo.contains(new_board)) {
                        queue.offer(new_board);
                        memo.add(new_board);
                    }
                }
            }
            step++;
        }
        return -1;
    }

    private String swap(char[] board, int adj, int idx) {
        char rep = board[adj];
        board[adj] = '0';
        board[idx] = rep;
        return String.copyValueOf(board);
    }
}
```

**Time Complexity:**
O(6!) factorial
**Space Complexity:**
O(6!) queue space, exponential board size
generalize to m*n board : O((m*n)!)

```
// generate m*n board neighbor
int[][] generateNeighborMapping(int m, int n) {
    int[][] neighbor = new int[m * n][];
    for (int i = 0; i < m * n; i++) {
        List<Integer> neighbors = new ArrayList<>();

        if (i % n != 0) neighbors.add(i - 1);
        
        if (i % n != n - 1) neighbors.add(i + 1);
        
        if (i - n >= 0) neighbors.add(i - n);

        if (i + n < m * n) neighbors.add(i + n);

        neighbor[i] = neighbors.stream().mapToInt(Integer::intValue).toArray();
    }
    return neighbor;
}
```
