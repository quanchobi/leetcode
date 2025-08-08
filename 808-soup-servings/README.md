# 808. Soup Servings

## First Thoughts

We don't need to worry about the probability of each choice at all, as they are each equally likely, independent, and we need to search each possibility anyway.

This seems like a recursion problem, where the base case is if A or B have less than or equal to 0 mL of soup remaining.

## First Attempt:

```cpp
// returns score 
double serve_soup(int mL_A, int mL_B) {
    // base case 1: tie
    if (mL_A <= 0 && mL_B <= 0)
        return 0.5;
    // base case 2: A wins
    if (mL_A <= 0)
        return 1.0;
    // base case 3: B wins
    if (mL_B <= 0)
        return 0.0;

    double result = 0.25* (
        // recursive case 1: pour 100 mL from A and 0 mL from B
        serve_soup(mL_A - 100, mL_B - 0) +

        // recursive case 2: pour 75 mL from A and 25 mL from B
        serve_soup(mL_A - 75, mL_B - 25) +

        // recursive case 3: pour 50 mL from A and 50 mL from B
        serve_soup(mL_A - 50, mL_B - 50) +

        // recursive case 4: pour 25 mL from A and 25 mL from B
        serve_soup(mL_A - 25, mL_B - 75) +
    );

    return result;
    
}
```

This was too slow. Like yesterday, I decided to attempt to use memoization to fix it, as there are a lot of unnecessary calculations.


```cpp
class Solution {
private:
    map<pair<int, int>, double> memo;
public:
    double serve_soup(int mL_A, int mL_B) {
        // base case 1: tie
        if (mL_A <= 0 && mL_B <= 0)
            return 0.5;
        // base case 2: A wins
        if (mL_A <= 0)
            return 1.0;
        // base case 3: B wins
        if (mL_B <= 0)
            return 0.0;

        pair<int, int> key(mL_A, mL_B);

        if (memo.find(key) != memo.end()) {
            return memo[key];
        }

        double result =
            0.25 * (
                       // recursive case 1: pour 100 mL from A and 0 mL from B
                       serve_soup(mL_A - 100, mL_B - 0) +

                       // recursive case 2: pour 75 mL from A and 25 mL from B
                       serve_soup(mL_A - 75, mL_B - 25) +

                       // recursive case 3: pour 50 mL from A and 50 mL from B
                       serve_soup(mL_A - 50, mL_B - 50) +

                       // recursive case 4: pour 25 mL from A and 25 mL from B
                       serve_soup(mL_A - 25, mL_B - 75));
        memo.add(key, result);
        return result;
    }
    double soupServings(int n) {
        return serve_soup(n, n);
    }
};
```

Again, this was too slow. Fortunately, because a will most likely get more removed from it than b, as n increases, the probability a will be emptied first asymptotically approaches 1.

Thus, if we can find a sufficiently large value of n such that the probability of a emptying first will be computed as 1, we can skip all of the resource consuming calculations.

After tinkering around with a few numbers, I settled on n=5000.

Thus, the final solution is

```cpp
class Solution {
private:
    map<pair<int, int>, double> memo;
public:
    double serve_soup(int mL_A, int mL_B) {
        // base case 1: tie
        if (mL_A <= 0 && mL_B <= 0)
            return 0.5;
        // base case 2: A wins
        if (mL_A <= 0)
            return 1.0;
        // base case 3: B wins
        if (mL_B <= 0)
            return 0.0;

        pair<int, int> key(mL_A, mL_B);

        if (memo.find(key) != memo.end()) {
            return memo[key];
        }

        double result =
            0.25 * (
                       // recursive case 1: pour 100 mL from A and 0 mL from B
                       serve_soup(mL_A - 100, mL_B - 0) +

                       // recursive case 2: pour 75 mL from A and 25 mL from B
                       serve_soup(mL_A - 75, mL_B - 25) +

                       // recursive case 3: pour 50 mL from A and 50 mL from B
                       serve_soup(mL_A - 50, mL_B - 50) +

                       // recursive case 4: pour 25 mL from A and 25 mL from B
                       serve_soup(mL_A - 25, mL_B - 75));
        memo.add(key, result);
        return result;
    }
    double soupServings(int n) {
        if (n > 5000) {
            return 1.0;
        }
        return serve_soup(n, n);
    }
};
```
