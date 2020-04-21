---
layout: post
title:  "Prefix Sum - The general approach to a contiguous subarray problem."
date:   "2020-04-13 12:00:00 -0400"
categories: jekyll update
---
## Introduction
-------
A prefix sum of an array of numbers is the cumulative sum of the array. Using this method, we can easily calculate the sum between two points of the array in constant time. I will be updating this page as and when I come across more prefix sum problems.

### 1. Find Subarray with given sum 
-------
**Description** 

Find if there exists a subarray with a given sum in an unsorted array.

**Explanation:** 
+ We will be using the two pointer approach - right and left pointers
+ Start with both pointers at index 0.
+ Get the cumulative sum as you iterate through the array
+ If the cumulative sum is less than target, increment right pointer
+ If the cumulative sum is greater than target, increment the left pointer and subtract the left out element from the cumulative sum.
+ When you reach a point where cumulative sum = target, we have the answer.

&nbsp;

### 2. LeetCode 560 - Subarray Sum equals K
-------
**Description:** 

Find the number of subarrays that exist with a given sum.
```
Eg:	Input:nums = [1,1,1], k = 2
	Output: 2
```

**Explanation:** 
+ As we iterate through the array, we calculate the prefix sum. We add prefix sum 0 to a map with the number of occurrences as 1.
+ So, for a given prefix sum(`cur_sum`), we know that there will be a subarray which sums to `k`, if and only if, there exists a prefix sum(`pre_sum`) already such that `(pre_sum + k = cur_sum)`. 
+ So we check the map to see if `pre_sum = (cur_sum - k)` exists. 
+ If it exists, we update the global counter. We then increment the value of `map[cur_sum]`.

```cpp
    int subarraySum(vector<int>& nums, int k) {
        int count = 0, sum = 0;
        unordered_map<int,int> pre_sum;
        pre_sum[0]++;
        for (int x : nums) {
            sum += x;
            if (pre_sum.find(sum - k) != pre_sum.end()) count += pre_sum[sum - k];
            pre_sum[sum]++;
      }
        return count;
    }
```
&nbsp;

### 3. Leetcode 523 - Continuous Subarray Sum where sum is a multiple of k
-------
**Description:** 

Given a list of non-negative numbers and a target integer k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to a multiple of k, that is, sums up to n*k where n is also an integer.

**Explanation:** 
+ The key here is that if the difference between the presums (sum of the contiguous segment) is divisible by k, then each of those pre sum values mod k should be equal
+ i.e, if `(b - a) % k == 0`, then `(b % k) == (a % k)`. 
+ This is why we store the mod value. We are storing the previous value because of the condition that subarray should be of at least size = 2. 
+ If `k = 0`, then we can't take mod k. So, we need to find two adjacent 0s to return true.

```cpp
bool checkSubarraySum(vector<int>& nums, int k) {

    int n = nums.size(), sum = 0, prev = 0;
    unordered_set<int> mod_set;
    for (int x : nums) {
        sum += x;
        int mod = k == 0 ? sum : sum % k;
        if (mod_set.find(mod) != mod_set.end()) return true;
        mod_set.insert(prev);
        prev = mod;
    }
    return false;
    
}
```

&nbsp;

### 4. Leetcode 525 - Longest Contiguous Binary Array
-------
**Description:** 

Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.

**Explanation:** 
+ Once again we calculate a prefix sum. But here, since the array is binary, we add -1 if the value is 0 and 1 if the value is 1.
+ Using this approach, we can calculate that there are equal number of 1s and 0s if the sum is 0.
+ Since we calculate the prefix sum from the beginning, when the sum is 0, we hav encountered equal 0s and 1s from the beginning. This will be the longest array.
+ But we can also have intermediate arrays which we will keep track of using a hash map.
+ Since we know that the maximum range of the values of prefix sum is -size to + size, we use an array for constant find() operation.
+ The array is initialized to -1 and will store the index of the first time the prefix sum occurred.
+ We add `size` to convert negative indexes (values of prefix sum) to positive.
+ We use the array to check when was the last time that current prefix sum occurred.
+ ` (max_sum = max(max_sum, i - arr[pre_sum + size] 

```cpp
int findMaxLength(vector<int>& nums) {
    int max_sum = 0, pre_sum = 0;
    int size = nums.size();
    vector<int> arr(size*2 + 1, -1);
    
    for (int i = 0; i < nums.size(); i++) {
        nums[i] ? pre_sum++ : pre_sum--;
        if (pre_sum == 0) max_sum = i + 1;
        else if (arr[pre_sum + size] == -1) arr[pre_sum + size] = i;
        else (max_sum = max(max_sum, i - arr[pre_sum + size]));
    }
    
    return max_sum;
}
```