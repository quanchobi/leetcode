# 3479. Fruits Into Baskets III

## Description:

This is the same as Fruits Into Baskets II, but with different constraints.

## Ideas:

We can attempt the same solution as last time.

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

However, this results in the time limit being exceeded. We need to find an solution better than O(n^2).

Ideas:

Break the large basket array into smaller subsets (square root decomposition), iterate over decomposed set of baskets, find first section that the fruit can fit in, find its index from there.

To store the largest value of each section, we can use an array with $\sqrt{n}$ entries, where each `sec[m]` being the largest value in the $mth$ section.

After a basket gets consumed, we need to update the maximum of that section.

## Solution:

```cpp
int numOfUnplacedFruits(vector<int>& fruits, vector<int>& baskets) {
    int total_overflow = 0;

    // create bucket of baskets (slightly confusing naming conventions lol)
    int size_baskets = baskets.size();
    int bucket_size = sqrt(size_baskets);

    int num_buckets = (size_baskets + bucket_size - 1) / bucket_size; 

    // initialize the max vector
    vector<int> bucket_max(num_buckets, 0)
    for (int i = 0; i < size_baskets; i++) {
        int bucket_index = i / bucket_size; // index relative to start of current bucket
        bucket_max.at(bucket_index) = max(bucket_max.at(bucket_index), baskets.at(i));
    }
    
    // Process each fruit
    for (int fruit_size: fruits) {
        bool fruit_placed = false;

        // find first bucket capable of holding fruit_size fruits
        for (int bucket = 0; bucket < num_buckets && !fruit_placed; bucket++) {
            // Skip buckets that are too small
            if (fruit_size > bucket_max.at(bucket)) {
                continue;
            }

            // implicit else; found bucket with big enough max.
            bool basket_found_in_bucket = false;
            int new_bucket_max = 0;
            int bucket_start = bucket * bucket_size;
            int bucket_end = min(bucket_start + bucket_size, size_baskets);

            // check baskets
            for (int i = bucket_start; i < bucket_end; i++) {
                // basket found
                if (baskets.at(i) >= fruit_size && !basket_found_in_bucket) {
                    baskets.at(i) = 0; // mark as used
                    basket_found_in_bucket = true;
                    fruit_placed = true;
                }

                // update bucket_max entry
                new_bucket_max = max(new_bucket_max, baskets.at(i));
            }

            bucket_max.at(bucket) = new_bucket_max;
        }
        if (!fruit_placed) {
            total_overflow++;
        }
    }
    return total_overflow;
}
```
