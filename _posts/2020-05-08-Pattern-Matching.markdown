---
title: "Pattern Matching - KMP and more"
tags: [strings, pattern, matching, knuth morris pratt, KMP]
style: fill
color: danger
description: This post documents important algorithms such as KMP that are used to solve string pattern matching problems.
---
### Index
-------
1. [Knuth Morris Pratt](#kmp)
2. [Problem 1 - Repeated substring Pattern 459](#prob1)
3. [Problem 2 - TBD](#prob2)

&nbsp;

### <a name="kmp"></a> Knuth Morris Pratt Algorithm
-------
The O(n^2) Naive pattern search algorithm can be optimized by using the KMP algorithm O(m+n).
The concept revolves around finding patterns in the substring that is to be found in the longer string and building an array based on these patterns. This array is then used when traversing the longer string to skip iterations, thus reducing the time complexity.

One of the best explanations out there for KMP is :-[http://jakeboxer.com/blog/2009/12/13/the-knuth-morris-pratt-algorithm-in-my-own-words/](http://jakeboxer.com/blog/2009/12/13/the-knuth-morris-pratt-algorithm-in-my-own-words/)

The code to construct the kmp array is:-
```cpp
string str = <match string>;
int i = 1, j = 0, n = str.size();
vector<int> kmp(n,0);
while( i < str.size() ){
    if( str[i] == str[j] ) kmp[i++]=++j;
    else if( j == 0 ) i++;
    else j = kmp[j - 1];
}
```

&nbsp;

### <a name="prob1"></a>  [Repeated substring Pattern 459](https://leetcode.com/problems/repeated-substring-pattern/)
-------
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**Input**: "abcabcabcabc"

**Output**: True

**Explanation**: It's the substring "abc" four times. (And the substring "abcabc" twice.)

This first solution in the discuss section absolutely blew my mind away. If you append the string to itself and remove the first element and last element, you should be able to find the string in this newly created substring because of the repeated pattern.
```cpp
    bool repeatedSubstringPattern(string str) {
        return (s + s).substr(1, s.size() * 2 - 2).find(s) != -1;
        /* or */
        return (s + s).find(s, 1) < s.size();
    }
```

The second solution uses KMP. Once you build the KMP array, the value of the final element should be `n - <length of repeated string>`
This also means that the value of the last element should be divisible by `n - <value of last element>`

```cpp
 bool repeatedSubstringPattern(string str) {
    int i = 1, j = 0, n = str.size();
    vector<int> kmp(n,0);
    while( i < str.size() ){
        if( str[i] == str[j] ) kmp[i++]=++j;
        else if( j == 0 ) i++;
        else j = kmp[j-1];
    }
    return kmp[n-1] && (kmp[n-1] % (n - dp[n-1]) == 0);
}
```

&nbsp;

### <a name="prob2"></a>
-------
