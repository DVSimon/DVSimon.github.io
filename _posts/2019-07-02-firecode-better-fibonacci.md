---
title: "Firecode 14: Better Fibonacci"
categories:
  - Firecode
tags:
  - firecode
  - python
---
The Fibonacci Sequence is the series of numbers: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ... The next number is found by adding up the two numbers before it.

Your goal is to write an optimal method - better_fibonacci that returns the nth Fibonacci number in the sequence. n is 0 indexed, which means that in the sequence 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ..., n == 0 should return 0 and n == 3 should return 2. Your method should exhibit a runtime complexity of O(n) and use constant O(1) space. With this implementation, your method should be able to compute larger sequences where n > 40.

Example:

```
better_fibonacci(0) ==> 0
```
```
better_fibonacci(1) ==> 1
```
```
better_fibonacci(3) ==> 2
```

## My Code

```python
def better_fibonacci(n):
    var1 = 0
    var2 = 1
    if (n == 0): return var1
    if (n == 1): return var2
    i = 2
    while i <= n:
        temp_var = var1+var2
        var1 = var2
        var2 = temp_var
        i +=1
    return var2
```

## Explanation

* Original fibonacci we did recursively.  return fib(n-2) + fib(n-1).  Since we want O(1) space complexity and O(n) time complexity we need to instead store these two in variables.  No matter the number computing for, we will always have the same space usage and a linear time complexity.
* Basic statements to return for n=0/n=1.
* Else loop from 2(outside of covered returns) till the input n and calculate the fibonacci's of each along the way and constantly replace the variables until the final variable is the fibonacci value of n.
