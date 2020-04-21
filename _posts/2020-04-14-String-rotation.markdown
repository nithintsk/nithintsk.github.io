---
layout: post
title:  "String Rotation and Reversal - Different Methods"
date:   "2020-04-14 12:00:00 -0400"
categories: jekyll update
---
## Introduction
-------
While rotation and left/right shifting with extra space is relatively easy, in place rotation is not as straightforward. Luckily, C++ provides us with its own algorithms to carry out these operations with relative ease. It is important, however, to remember the syntax of std::rotate and std::reverse. There is one more trick we can use to carry out string rotation using the reverse function. All this and more are explained below.

### 1. Reversing a string or container
-------

There are two methods to reverse a string:

#### 1. We write our own simple in place reversal function.
+ The function takes two arguments:- A reference to the string and the starting and ending index `[start, end)`, between which the characters need to be rotated.


```cpp
int reversal(string &s, int start, int end) {
    if (start < 0 || start >= s.size()) return -1;
    if (end <= 0 || end > s.size()) return -1;
    if (start > end) return -1;
    if (start == end) return 0;
    for (int i = start, j = end - 1; i < (start + end) /2; i++, j--) {
        // Swapping without an extra variable
        s[i] = s[i] + s[j];
        s[j] = s[i] - s[j];
        s[i] = s[i] - s[j];
    }
    return 0;
}
```

#### 2. We use <algorithm> library's std::reverse function.
+ The function of std::reverse is to reverse the order of the elements in the range [first, last) of any container.
+ The syntax is `void std::reverse(Bidirectional iterator first, Bidirectional iterator end)`.
+ Since the arguments take bidirectional iterators, we can use rbegin and rend too.

```cpp
string s = "HelloWorld";
std::reverse(begin(s), end(s));
std::reverse(begin(s) + 1, end(s) -2);
```

&nbsp;

### 2. Rotation of a string or container
-------

Rotation can happen in either direction - left or right. It is important to remember that rotating left by `n`, is the same as rotating right `size - n` times. We can use the reverse function to rotate an array. This is a shortcut when we can't use the std::rotate function. The three methods to rotate a container are described below. 

#### 1. When we can't use std::rotate, but have our reverse function.
+ In this method, to left rotate by n, we first reverse the range `[start, n)` and `[n, end)`.
+ Then we rotate the whole container.
+ To right rotate, we just need to call left rotate with `n = size - n`

```cpp
void left_rotate(string &s, int n) {
    reversal(s, 0, n);
    reversal(s, n, s.size());
    reversal(s, 0, s.size());
}

void right_rotate(string &s, int n) {
    left_rotate(s, s.size() - n);
}
```

#### 2. Using std::rotate
+ The syntax of the function is `std::rotate(forward iterator start, forward iterator middle, forward iterator end);`
+ Start and end are forward iterators of the container to be rotated
+ Middle is going to become the first element.
+ Essentially, it is left rotate until middle is at the beginning of the array

```cpp
// Left rotate by n
std::rotate(begin(s), begin(s) + n, end(s));

// Right rotate by n
std::rotate(begin(s), begin(s) + (s.size() - n) , end(s));
```

####  3. Using std::rotate_copy
+ The syntax of the function is `std::rotate(forward iterator start, forward iterator middle, forward iterator end, destination iterator start);`
+ Same as rotate but it copies the rotated segment to the new container at destination iterator start

```cpp
int arr[] = { 1, 2, 3, 4, 5, 6, 7 , 8, 9, 10};  
vector<int> dest(10);
std::rotate_copy(arr, arr + 3, arr + 10, begin(dest));
// Dest would now contain: 4, 5, 6, 7, 8, 9, 10, 1, 2, 3
```
&nbsp;
