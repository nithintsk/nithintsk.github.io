---
title: Bit arithmetic - Cheat Sheet and Programs
tags: [bit arithmetic, bit, xor, operators, bitwise, Leetcode]
style: fill
color: info
description: This is a cheatsheet for common bitarithmetic problems on Leetcode. This sheet will continuously be updated whenever I find amusing new bit arithmetic problems.
---

### Index
-------
1. [List of Bitwise operators](#operators)
2. [Tips and tricks](#tips)
3. [Bitwise hacks for competitive programming](https://www.geeksforgeeks.org/bitwise-hacks-for-competitive-programming/)
4. [Problem 1 - Bitwise AND of a range of numbers](#prob1)
5. [Problem 2 - Ones complement of a number](#prob2)
6. [Problem 3 - Count the number of 1s in numbers in range 0 to num inclusive](#prob3)
7. [Problem 4 - Find a missing number in the range 0 to n](#prob4)


&nbsp;

### <a name="operators"></a> List of Bitwise operators
-------

| AND         | **&**  
| OR          | **\|**  
| XOR         |  **^**  
| Left shift  |  **<<** 
| Right Shift |  **>>** 
| NOT         |  **~**  

&nbsp;

### <a name="tips"></a> Important tips and tricks to remember (the non obvious ones)
-------
**Right shift by 1 is division by two. Left shift by 1 is multiplication by 2 (It's obvious. I know.)**
```cpp
a = 12 >> 1; // a gets value of 6
a = 12 << 1; // a gets value of 24
```

**Bitwise AND(`&`) to check if even or add. Checks the LSB (0 is even, 1 is odd)**
```cpp
if (num & 1) cout<<"ODD";
else cout<<"EVEN";
```

**Convert a character to lowercase regardless of what case it is in**
```cpp
ch = ch | ' '
```

**Convert a character to uppercase regrdless of what case it is in**
```cpp
ch = ch & '_'
```

**Swap two variables without a third - using XOR**
```cpp
a = a^b;
b = b^a; // same as b = b ^ a ^ b
a = a^b; // same as a = (a ^ b) ^ (b ^ a ^ b)
```

&nbsp;

### [Bitwise hacks for competitive programming](https://www.geeksforgeeks.org/bitwise-hacks-for-competitive-programming/)

&nbsp;

### <a name="prob1"></a> Bitwise AND of a range of numbers
-------
The result will be 0 if:- `log2(n/m) != 0` <--> `log2(n) != log2(m)`. In the below method, m will be equal to n and the loop will break only if the MSB of m and n are the same.
```cpp
int rangeBitwiseAnd(int m, int n) {
    int count = 0;
    while(m != n){
        m >>= 1;
        n >>= 1;
        count += 1;
    }
    return m <<= count;
}
```

&nbsp;

### <a name="prob2"></a> 1s complement of a number
-------
**Eg. 1** Input: 5 Output: 2

Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

**Eg. 2** Input: 1 Output: 0

Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.

```cpp
int findComplement(int num) {
    int mask = 1;
    while (mask < num) {
        mask = (mask << 1) + 1;
    }                                                                    
    return mask ^ num;
}
```

&nbsp;

### <a name="prob3"></a> Get count of 1s of all numbers in range \[0, num\]
-------
**Eg. 1** Input: 5 Output: [0,1,1,2,1,2]

We can use inbuilt function: `cout<< __builtin_popcount (4);`

This is a form of dynamic programming, where we use the value of res[i/2] which
will have the same number of 1s as res[num] + (1 if odd, 0 if even)

```cpp
vector<int> countBits(int num) {
    vector<int> res(num + 1, 0);
    for (int i = 0; i < num + 1; i++) {
        res[i] = res[i >> 1] + (i & 1);
    }
    return res;
}
```

&nbsp;

### <a name="prob4"></a> Find missing number in an array of n elements in the range of 0 to n.
-------
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

**Eg. 1** Input: [3,0,1] Output: 2

**Eg. 2** Input: [9,6,4,2,3,5,7,0,1] Output: 8

```cpp
public int missingNumber(int[] nums) { //xor
    int res = nums.length;
    for(int i=0; i<nums.length; i++){
        res ^= i;
        res ^= nums[i];
    }
    return res;
}
bit arithmetic, bit, xor, operators, ```
