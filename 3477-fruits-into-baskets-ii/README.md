# 3477. Fruits Into Baskets II

## Description:

This one is pretty simple.
For each fruit type, we go from left to right checking each basket. At the first basket we find that all fruit of that type fits in, we mark the basket as having 0 size such that no other fruit tries to fill and break to go to the next fruit type. If we make it to the end of the basket list, then we increment the total number of fruit types without a basket.

## Solution:

```cpp
int numOfUnplacedFruits(vector<int>& fruits, vector<int>& baskets) {
    int total_overflow = 0;
    for (int i = 0; i < fruits.size(); i++) {
        for (int j = 0; j < baskets.size(); j++) {
            // fill basket (set to 0 such that no other fruit tries to fill it)
            if (fruits.at(i) <= baskets.at(j)) {
                baskets.at(j) = 0;
                break;
            }

            // If made it to the end of the list without a fill, add one to overflow
            if (j == baskets.size() - 1) {
                total_overflow++;
            }
        }
    }
    return total_overflow;
}
```
