# 5. Longest Palindromic Substring

[link](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string `s`, return the longest palindromic substring in `s`.

## Naive Idea:

There are 2 possible palindrome types: even and odd. These require different checks.

For odd palindromes at `i, i`. Move pointers outward until they do not match.
For even palindromes at `i, i+1`. Move pointers outward until they do not match.

iterate over the palindrome, check for palindromes starting at i.

```cpp
string longestPalindrome(string s) {
    int n = s.size();
    int longest_size = 0;
    for (int i = 0; i < n; i++) {
        // odd check
        int j = i;
        int k = i;
        int odd_curr_size = 1;
        while (s.at(j) == s.at(k) && j >= 0 && k <= n) {
            odd_curr_size += 2;
            j--;
            k++;
        }

        // even check
        int j = i;
        int k = i + 1;
        int even_curr_size = 0;
        while (s.at(j) == s.at(k) && j >= 0 && k <= n) {
            even_curr_size += 2;
            j--;
            k++;
        }
        
        // length check
        if (odd_curr_size > longest_size) {
            longest_size = odd_curr_size;
        }

        if (odd_curr_size > longest_size) {
            longest_size = even_curr_size;
        }
    }
}
```

## Attempt 2:

I misunderstood the assignment. I need to return the longest *string*, not the length of the longest string.

```cpp
string longestPalindrome(string s) {
    int n = s.size();
    string ans = "";

    for (int i = 0; i < n; i++) {
        // odd check
        int j = i;
        int k = i;

        while (j >= 0 && k < n && s.at(j) == s.at(k)) {
            j--;
            k++;
        }
        string odd = s.substr(j + 1, k - j - 1);

        if (odd.size() > ans.size())
            ans = odd;

        // even check
        j = i;
        k = i + 1;
        while ( j >= 0 && k < n && s.at(j) == s.at(k)) {            
            j--;
            k++;
        }
        string even = s.substr(j + 1, k - j - 1);

        // size check
        if (even.size() > ans.size())
            ans = even;
    }
    return ans;
}
```
