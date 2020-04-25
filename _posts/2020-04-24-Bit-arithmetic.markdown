---
layout: post
title: "Bit arithmetic - Cheat Sheet and Programs"
date:   "2020-04-24 12:00:00 -0400"
categories: jekyll update
---
### Index
-------
1. [List of Bitwise operators](#operators)
2. [Tips and tricks](#tips)
3. [Bitwise hacks for competitive programming](https://www.geeksforgeeks.org/bitwise-hacks-for-competitive-programming/)
4. [Problem 1 - Bitwise AND of a range of numbers](#prob1)

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
    while(m!=n){
        m>>=1;
        n>>=1;
        count+=1;
    }
    return m<<=count;
}
```