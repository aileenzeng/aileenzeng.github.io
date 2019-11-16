---
layout: post
type: blog
categories: [Interview Prep]
title: Sort Colors Walkthrough
featured-img: posts/emile-perron-190221
img-type: jpg
mathjax: true
---


This is a detailed walkthrough of the Sort Colors problem from Leetcode. 
(Link [here](https://leetcode.com/problems/sort-colors/)). This is based
off an email that I sent to my section earlier about how I like to
approach technical questions.

## Question
Given an array with n objects colored red, white or blue, sort them in-place
so that objects of the same color are adjacent, with the colors in the order 
red, white and blue.
 
Here, we will use the integers 0, 1, and 2 to represent the color red, white, 
and blue respectively.
 
Note: You are not supposed to use the library's sort function for this problem.
 
Example:
```
Input:  [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

## Walkthrough of Optimized Solution
I'll assume that you all understand the problem at the step, so I'm going
to talk about my thought process starting from **identifying sub-problems /
sub-tasks**.

I'm just going to be doing a walkthrough of the optimized version, which
completes this problem in a single pass and uses constant space. In other
words, the time complexity is $$\Theta(n)$$ and the space complexity is 
$$\Theta(1)$$, where $$n$$ is the size of the input array.

### Identifying Sub-Problems and Sub-Tasks
Since I'm trying to do this in place, I think one sub-task that I will need
to perform is a swap in an array. I'll probably want some helper method to do
this since swap code is a little verbose, and I don't want it to clutter up
my main solution:

```java
/**
 * Swaps items in an array.
 * @param a Index of the first element to swap.
 * @param b Index f the second element to swap.
 */
private void swap(int a, int b, int[] nums);
```

### Skeleton & Invariants (the hard part)
My first thought is that I'll need to iterate through the array somehow to
partition all the elements. I'm not exactly sure what my conditions are going
to be yet, but I think that somehow I'll need to keep track of an index for
my current location, and that if I'll need to do something different when I 
encounter a 0, 1 or 2.

I also want to keep track of my partitions. The `first` partition is going to
be the index of the last 0, and the `second` partition is going to be the 
index of the first 2. It doesn't really matter what my rules for my partitions
are, as long as I'm consistent with them.

The idea is that as I'm progressing through `nums`, I'm going to move the 
`first` partition forwards as I encounter 0s, and I'm going to move the
`second` partition backwards as I encounter 2s.

I can't be sure if there are going to be any 0s in the list, so I'm going to
set `first` to `-1`. I also can't be sure if there are any 2s in my list,
so I'm going to set `second` to `nums.length`.

I'm not exactly sure how I'm handling these cases yet; I just know that I'll
have to handle them eventually, so I'll write down the code now:

```java
class Solution {
    public void sortColors(int[] nums) {
        int first = -1;             // The index of my last 0.
        int second = nums.length;   // The index of my first 2.
        int index = 0;              // The index that I'm currently exploring.
        while (/* some condition */) {
            if (nums[index] == 0) {
                // Do stuff
            } else if (nums[index] == 1) {
                // Do some other stuff
            } else {
                // Do other stuff
            }
        }
    }

    /**
     * Swaps items in an array.
     * @param a Index of the first element to swap.
     * @param b Index f the second element to swap.
     */
    private void swap(int a, int b, int[] nums) {
        // something here
    }
}
```

To recap, here are the rules (invariants) I'm making for my solution:
* `first` is the index of the last 0 that I have encountered. In other words,
    all numbers in `nums[0..first]` are 0.
* `second` is the index of the first 2 that I have encountered. In other words,
    all numbers in `nums[second..nums.length-1]` are 2.
* `index` is the index that we're currently analyzing. When we entire the 
    `while` loop, we know that the elements from `nums[0..index-1]` have been 
    placed correctly.
* All numbers from `nums[first..index-1]` are 1. This is probably the trickiest
    invariant to come up with (but is an important one!). If we know that all
    elements placed from `nums[0..index-1]` are correct and that all elements
    from `nums[0..first]` are 0, then the remaining elements from 
    `nums[first+1..index-1]` must be 1.

### Filling in the code
I like to fill in my code starting with the helper functions, and work my way 
from easy problems to hard problems. When I'm writing loops, I also like to 
write the loop body first (based off the invariants I created in the earlier 
step), and then write the condition later.

#### Writing Swap
First: I'm going to write `swap`. Once I've written this helper method, I can 
forget how it works and just assume that it does what it promises.

```java
    /**
     * Swaps items in an array.
     * @param a Index of the first element to swap.
     * @param b Index f the second element to swap.
     */
    private void swap(int a, int b, int[] nums) {
        int temp = nums[a];
        nums[a] = b;
        nums[b] = temp;
    }
```

#### Writing the while loop body
Next, I'm going to tackle each of the cases for when 
`nums[index] = 0, 1, or 2`.
* When `nums[index] == 0`: I want to swap the 0 that I encountered here to be
    with the rest of the 0s. I know that all the elements from 
    `nums[0..first]` are guaranteed to be 0. I don't know what 
    `nums[first + 1]` is though (but I know that it's next to the 0s), so I 
    should swap `nums[index]` with the element at `nums[first + 1]`. To 
    maintain the `first` invariant, I should increase my partition for `first`
    because I found a new 0. I should also explore the next index, so I should
    increment `index`.
* When `nums[index] == 2`: I want to swap the 2 that I encountered here to be
  with the rest of the 2s. I know that all the elements from 
  `nums[second..nums.length-1]` are guaranteed to be 2. I don't know what
  `nums[second - 1]` is though (but I know that it's next to the 2s), so I
   should swap `nums[index]` with the element at `nums[second - 1]`. To 
   maintain the `second` invariant, I should decrease my partition for 
   `second` because I found a new 2.
* When `nums[index] == 1`: This doesn't affect any of my `first` or `second` 
  invariants, so I'm not going to touch those. However, it does increase our
  "known" territory, so we should increase `index`.

This covers all the cases in my `while` loop. 
```java
    while (/* some condition */) {
        if (nums[index] == 0) {
            swap(index, first + 1, nums);
	        first++;
            index++;
        } else if (nums[index] == 1) {
            index++;
        } else {
            swap(index, second - 1, nums);
            second--;
        }
    }
```

#### Writing while loop condition
From my invariants earlier, I know that these statements must be true:
* All elements from `nums[0..index]` must be placed correctly.
* All elements from `nums[second..nums.length-1]` must be placed correctly.

`index` is the element that we're using to "explore". Once `index == second`,
we're done! When `index == second`, the following statements are true:
* All elements from `nums[0..second]` must be placed correctly.
* All elements from `nums[second..nums.length-1]` must be placed correctly.

Therefore, all elements from `nums[0..nums.length-1]` must be placed correctly,
which is exactly what we wanted :)

```java
    while (index < second) {
        ...
    }
```

## Final solution
```java
class Solution {
    public void sortColors(int[] nums) {
        int first = -1;
        int second = nums.length;
        int index = 0;
        
        while (index < second) {
            if (nums[index] == 0) {
                swap(index, first + 1, nums);
                first++;
                index++;
            } else if (nums[index] == 1) {
                index++;
            } else {
                swap(index, second - 1, nums);
                second--;
            }
        }
    }
    
    private void swap(int a, int b, int[] nums) {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```

