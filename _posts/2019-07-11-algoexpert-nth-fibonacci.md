---
title: "Algoexpert: Nth fibonacci"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def getNthFib(n):
  # Write your code here.
    #soln 1
	# if n == 2:
	# 	return 1
	# elif n == 1:
	# 	return 0
	# else:
	# 	return getNthFib(n-1) + getNthFib(n-2)
    #soln2
	# mem = {2:1, 1:0}
	# if n in mem:
	# 	return mem[n]
	# else:
	# 	mem[n] = getNthFib(n-1) + getNthFib(n-2)
	# 	return mem[n]
	 #third soln
	if n == 2:
		return 1
	if n == 1:
		return 0
	temp_fib_count = n
	i = 2
	temp_fibs = [0,1]
	next = None
	while temp_fib_count > i:
		next = temp_fibs[0] + temp_fibs[1]
		temp_fibs = [temp_fibs[1], next]
		i+=1
	return next

```

## Explanation

* First solution uses basic recursion but isn't time efficient.
* Second solution O(n) time and O(n) space. stores past fib calculations in a dictionary(cache) and uses it to return fibonacci or calcuate new fibonaccis for a constant time operation.
* Third solution O(n) time and O(1) space so it's most efficient throught iteration.  Uses a counter to count iterations while storing the past 2 fibonacci(n-2,n-1) in a list which constantly updates itself with the next fibonacci term until reaching the input value/count.
