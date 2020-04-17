---
layout: post
title:  "Deleting duplicates in std::vector"
date:   2020-04-16 21:48:00 -0400
categories: jekyll update
---
## Introduction
-------
This post details different methods - from the least efficient to the most efficient - to delete duplicate elements from a `std::vector`. The methods described are aimed at vectors storing primitive data types like `int`, `char`, `float`, etc. The 5 methods that are described below are taken from [here](https://stackoverflow.com/questions/1041620/whats-the-most-efficient-way-to-erase-duplicates-and-sort-a-vector) and they are:-
1. [Using std::sort + std::unique](#sort_unique)
2. [Convert to a set using a constructor](#ordered_constructor)
3. [Convert to a set using manually](#ordered_manual)
4. [Convert to an unordered set using a constructor](#unordered_constructor)
5. [Convert to an unordered set manually - The fastest of them all](#unordered_manual)

### <a name="sort_unique"></a>Remove duplicates using std::sort and std::unique
-------
We first need to know the syntax of `std::sort`, `std::unique` and `std::vector::erase`.
1. `std::sort` sorts the elements in the range `[first, last)` in ascending order. Its syntax is `std::sort(begin(vec), end(vec))` where begin and end are `std::begin` and `std::end` respectively. The time complexity is `O(NlogN)`
&nbsp;

2. `std::unique` is an STL algorithm that provides the functionality to remove all **consecutive** duplicate elements from a given range. Hence, `std::sort` precedes `std::unique`. But it is important to remember that `std::unique` will not decrease the actual size of given range i.e. it will only overwrite the duplicate elements and returns an iterator to the new end of the range. **Hence, we need to erase the rest of the array.**
    Eg. 1,2,3,3,3,4,4,5,5 becomes 1,2,3,4,5,4,4,5,5
&nbsp;

3.  `std::vector::erase` is a member function of the vector datatype. It can be used to delete an individual element or a range of elements in the vector. 
+ `iterator erase (iterator position);`
+ `iterator erase (iterator first, iterator last);`

By combining these three operations, we can delete all duplicate elements in an array.

```cpp
vector<int> vec({1,1,3,2,5,4,5,5,2});
sort(begin(vec), end(vec));
vec.erase(unique(begin(vec), end(vec)), end(vec));
// vec will now be: 1, 2, 3, 4, 5
```

&nbsp;

### <a name="ordered_constructor"></a>Convert to a set using a constructor
-------
This method like the next few are relatively simple and intutive. The vector is copied into a set. A set can only contain unique elements. Any duplicates that are inserted are ignored. We then convert the set back into vector using `std::vector::assign` which will replace the existing vector. We have to remember that `std::set` is ordered, hence insertion will take `O(NlogN)` time at worst. However, this is still better than using sort + unique.

```cpp
vector<int> vec({1,1,3,2,5,4,5,5,2});
set<int> s(begin(vec), end(vec));
vec.assign(begin(s), end(s));
// vec will now be: 1, 2, 3, 4, 5
```

&nbsp;

### <a name="ordered_manual"></a>Convert to a set manually
-------
The issue with using a constructor is it adds some additional overhead. The author of the stack overflow comment, which this post is based on, discovered that the constructor of `std::set` and `std::unordered_set` actually constructs a new node for every element, before checking its value to determine if it should actually be inserted. This can largely be avoided by populating the set manually.

```cpp
vector<int> vec({1,1,3,2,5,4,5,5,2});
set<int> s;
for (int i : vec)
    s.insert(i);
vec.assign( s.begin(), s.end() );
// vec will now be: 1, 2, 3, 4, 5
```

&nbsp;

### <a name="unordered_constructor"></a>Convert to a unordered_set using a constructor
-------
`std::unordered set` as the name suggests does not store the elements in ascending order like `std::set` does. Hence, insertion times will be far better.

```cpp
vector<int> vec({1,1,3,2,5,4,5,5,2});
unordered_set<int> s(begin(vec), end(vec));
vec.assign( s.begin(), s.end() );
// vec will now be: 1, 2, 3, 4, 5
```

&nbsp;

### <a name="unordered_manual"></a>Convert to a unordered_set manually
-------
Finally we arrive at the fasted performing method. Using an unordered_set and avoiding the overhead caused by the constructor. This method performs much faster than the others mentioned above even as the size of vector increases.

```cpp
vector<int> vec({1,1,3,2,5,4,5,5,2});
unordered_set<int> s;
for (int i : vec)
    s.insert(i);
vec.assign( s.begin(), s.end() );
// vec will now be: 1, 2, 3, 4, 5
```
&nbsp;