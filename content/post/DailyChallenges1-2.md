---
title: "DailyChallenges 1 & 2"
date: 2019-10-07T21:27:17-04:00
draft: false
toc: false
comments: false
categories:
- Programming
tags:
- Daily Challenges
---

There is this website, Daily Coding Problem, that emails you Daily Challenges asked by major tech companies that you can complete for practice. Many people use it for coding interviews, but I'm aiming to use it to stop from getting rusty while I figure out what project I want to do next. Enjoy my thought processes!
<!--more-->
The first problem was recently asked by Google and is considered easy compared to other coding problem:

Given a list of numbers and a number `k`, return whether any two numbers from the list add up to `k`. For example, given `[10, 15, 3, 7]` and `k` of `17`, return true since `10 + 7` is `17`. Bonus: Try to solve this problem in only one pass.

Let's go for the least-efficient solution first for an easy answer and go from there. We know you can brute-force the algorithm and make the algorithm take O(n^2) time, which is the worst case, by using two for-loops to compare each number to all other numbers in the list. Good to know, let's not do that.

We could also subtract each number, `n`, from `k` and see if the result is in the list, returning `True` once we find a pair where `result + n = k` using Python3's `in` keyword. The issue with the `in` keyword is that, for lists, [the operation takes O(n) time] (https://stackoverflow.com/questions/13884177/complexity-of-in-operator-in-python), which is not ideal. But, let's assume the users did the smart thing and converted the list to a set, since the `in` operator performs in O(1) time for sets and dictionaries. Therefore, we can say the algorithm runs in O(n) time complexity since we have to look through the entire list for the worst case due to there being no possible pairs that add up to `k` in the worst case. So here is the algorithm we have:

    def problem(arr, k):
        # If the list size is less than two then there are no pairs.
        if len(arr) < 2:
            return False

        for number in arr:
            # See if the current number has a pair in the array.
            if (k - number) in arr:
                return True

        # Did not find any pairs  
        return False

To be fancy, let's make a list comprehension instead of the for-loop:

    def problem(arr, k):
        # If the list size is less than two then there are no pairs.
        if len(arr) < 2:
            return False

        return any(k - number in arr for number in arr)

The `any` keyword returns `True` on the first occurrence of `True`, allowing us to turn our above for-loop into this list comprehension! I took me awhile to figure out how to use a list comprehension in this way, but anything is possible with Python3 it seem! Onto the second problem.

Given an array of integers, return a new array such that each element at index `i` of the new array is the product of all numbers in the original array except the one at `i`.

Example: `function([1, 2, 3, 4, 5]) -> [120, 60, 40, 30, 24]`.

We could obviously brute-force the algorithm and get a O(n^2) time complexity, which is the worst case, by yet again using a double for-loop iterate through the list and getting the products for each position. But, I think there is a more subtle way to approach this problem.

We can see that `function(5) -> 24` is simply the first four numbers in the array multiplied together. We can also see that `function(1)` is simply the last four numbers in the array multiplied together.

Let's look at the numbers in between, we can see that `function(3) -> 40` since `1*2*4*5` equals `40`, but how can we do this algorithmically?

We can break `1*2*4*5` into two parts, the product to the left of `3`'s position multiplied by the product to the right of `3`'s position: `(1*2)*(4*5)`.

Let's do the same with `2` and `4`. For `2` we have, `(1) * (3*4*5)`; for `4` we have `(1*2*3)*(5)`. Now, looking at all of these possibilities, we can see a discernable pattern:

    f(1) -> () * (2 * 3 * 4 * 5)
    f(2) -> (1) * (3 * 4* 5)
    f(3) -> (1*2)*(4*5) 
    f(4) -> (1*2*3)*(5)
    f(5) -> (1*2*3*4) * ()

We can see that the products of the current number build off of the numbers to its left and right, meaning we need to calculate the products of both the numbers to a number's left and the numbers to a number's right and then multiply them together to get the number's final product. This means that the task can be accomplished in two passes, giving it a time complexity of O(2n). We are going to assume that the array is at least of `size 2` so we can actually have a product for each number in the array since there was no information in the problem to explain what to do with an array with a `size less than 2`.

    def problem(arr):
    
        n = len(arr)

        initialProduct = 1
    
        # Must initialize an array with 1's so products can be solved for without equaling 0.
        prodArr = [1] * n

        # Assign the product of the numbers to the left of the current number at the position of the current number in the array.
        # We can do this by storing the previous position's number (1 for the leftmost number), which is the product of all the previous numbers and put that in the current position.
        for i in range(n):
            prodArr[i] = initialProduct # No need to multiple since every number in prodArray is 1 until we give it initialProduct's current value. 
            initialProduct *= arr[i]    # Get the product of the numbers to the next number's left

        # Now we do the same thing, but going right-to-left instead of left-to-right.
        for i in range(n - 1, -1, -1):
            prodArr[i] *= initialProduct
            initialProduct *= arr[i]

        return prodArr
      
There we have it, an algorithm with O(2n) time complexity and O(n) space complexity. Not too bad.