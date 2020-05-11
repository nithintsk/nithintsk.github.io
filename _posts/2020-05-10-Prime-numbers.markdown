---
layout: post
title: "Finding Prime Numbers"
date:   "2020-05-10 12:00:00 -0400"
categories: jekyll update
---
### Index
-------
#### 1. [Sieve of Eratosthenes](#soe)
#### 2. [Problem 1 - Counting prime numbers until N 204](#prob1)
-------

&nbsp;

### <a name="soe"></a> Sieve of Eratostheneses
-------
The sieve of Eratosthenes is one of the most efficient ways to find all primes smaller than n when n is smaller than 10 million or so.(Ref: Wikipedia)

+ We create a list representing the range 2 to N. Now, starting from 2, we mark all the multiples of 2, which are greater than 2^2 as not prime. 
+ Then we move on to the next unmarked number in the list(3). We now mark all multiples of 3 which are greater than 3^2 as not prime.
+ We keep keep doing this till we reach a number p where p^2 is greater than N.
+ All the unmarked numbers left in the list are prime.

&nbsp;

### <a name="prob1"></a> [Counting prime numbers until N - 204](https://leetcode.com/problems/count-primes/)
-------
Count the number of prime numbers less than a non-negative number, n.

**Input**: 10
**Output**: 4
**Explanation**: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

We use the sieve. The first for loop is to visit each number and check if it is marked or unmarked.
The second for loop runs only if a number is unmarked(prime). It marks all multiples of i greater than i^2 as marked.

```cpp
int findPrimes(int N) {
    if (N < 2) return 0;
    int *arr = new int[N+1] {0};
    int count = 0;
    for (int i = 2; i < N; i++) {
        if (arr[i] == 0) {
            count++;
            for (int j = i; j <= N/i; j++) arr[j * i] = 1; 
        }
    }
    return count;
}
```
&nbsp;

### <a name="prob2"></a>
-------
Will append problems as I encounter them.