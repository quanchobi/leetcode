# Leetcode 898 - Bitwise ORs of Subarrays:

[link](https://leetcode.com/problems/bitwise-ors-of-subarrays/editorial/?envType=daily-question&envId=2025-07-31)

## Naive Idea:
Find each subarray in a doubly nested for loop, |= each value in that subarray in another for loop, store that value in a hash set. return hashset.size().

```cpp
int subarrayBitwiseORs(vector<int>& arr) {
    int n = arr.size();
    unordered_set<int> vals;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int val = 0;
            for (int foo = i; foo < j + 1; foo++) {
                val |= arr.at(foo);
            }
            vals.insert(val);
        }
    }
    return vals.size();
}
```

### Complexity:

#### Time: O(n^3)
Triply nested for loop. Pretty self explanatory.

#### Storage: O(1)
Extra storage needed for hash set.

### First Optimization Idea:
We can remove the innermost loop, because this still goes over all of the possible subarrays.

```cpp
int subarrayBitwiseORs(vector<int>& arr) {
    int n = arr.size();
    unordered_set<int> vals;
    for (int i = 0; i < n; i++) {
        int val = 0;
        for (int j = i; j < n; j++) {
            val |= arr.at(j);
            vals.insert(val);
        }
    }
    return vals.size();
}
```

This incrementally finds the OR value of all subarrays from i to j. The OR value of the subarray i to foo + i is the same as the OR value of i to foo OR'd with the value of foo + 1. No need to recompute each time as I was before.

Unfortunately, this optimization is still not quite fast enough for leetcode's standards.

### Thoughts:

Once a value becomes a 1, it can never go back to 0.

### Idea 2:
Iterate over array once,

Insert current num into a set (this is the same as subarray of itself)

Iterate over all previously found OR values and OR them with the current number, store this result in curr

set prev to curr for next loop

add curr to result

```cpp
unordered_map<int> prev;
unordered_map<int> result;

for (num: arr) {
    unorderd_map<int> curr;
    curr.insert(num);

    for (prev_val: prev) {
        curr.insert(prev_val | num);
    }

    prev = curr;
    result.insert(curr.begin(), curr.end());

    return result.size();
}
```
