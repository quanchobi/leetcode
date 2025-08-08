# 3363. Find the Maximum Number of Fruits Collected

[link](https://leetcode.com/problems/find-the-maximum-number-of-fruits-collected/description/?envType=daily-question&envId=2025-08-07)

## Ideas:

### Key insights:

1. The first child _must_ follow the diagonal, otherwise it would take more than n-1 turns

2. The second and third child cannot cross the diagonal, as it would take more than n-1 turns
   - This implies they also shouldn't enter the diagonal, as they won't get any points from it.

Combining the facts above, this means that the children cannot ever enter a space that another one has been to. This greatly simplifies the problem.

## Algorithm:

- follow the diagonal to find the score of child 1
- use DFS to maximize the score of child 2 and child 3
- add all three together and retunr the sum

## Code for Child 1:

```cpp
int n = fruits.size(); // grid is defined as nxn

int max_fruits = 0;

for (int i = 0; i < n; i++) {
    max_fruits += fruits.at(i).at(i);
}
```

## Code for child 2/3

```cpp
int dfs(int row, int col, int movesLeft, vector<vector<int>>& grid) {
    int n = grid.size();
    // check if child reached end (base case, return grid[row][col] or INT_MIN if there are moves left). Will be different for child 1 and child 2.
    // check invalid states (out of bounds, out of moves, return INT_MIN). Will be different for child 1 and child 2

    // recursively move closer to end (different for child 1 and child 2), check all possibilities (listed in problem statement).

    // return grid.at(i).at(j) + maxFruits where maxFruits is found from previous recursion.
}
```
