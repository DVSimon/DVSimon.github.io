---
title: "Leetcode - Roman to Integer"
categories:
  - Leetcode
tags:
  - leetcode
  - python
---
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

    I can be placed before V (5) and X (10) to make 4 and 9.
    X can be placed before L (50) and C (100) to make 40 and 90.
    C can be placed before D (500) and M (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

Example:

```
Input: "III"
Output: 3
```
```
Input: "IV"
Output: 4
```
```
Input: "IX"
Output: 9
```
```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```
```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## My Code

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        number = 0
        for idx, char in enumerate(s):
            if char == 'I':
                try:
                    if s[idx+1] == 'V' or s[idx+1] == 'X':
                            number-=1
                    else:
                        number+=1
                except IndexError:
                    print('last ele in list.')
                    number+=1
            if char == 'X':
                try:
                    if s[idx+1] == 'L' or s[idx+1] == 'C':
                            number-=10
                    else:
                        number+=10
                except IndexError:
                    print('last ele in list.')
                    number+=10
            if char == 'C':
                try:
                    if s[idx+1] == 'D' or s[idx+1] == 'M':
                            number-=100
                    else:
                        number+=100
                except IndexError:
                    print('last ele in list.')
                    number+=100
            if char == 'V':
                number+=5
            if char == 'L':
                number+=50
            if char == 'D':
                number+=500
            if char == 'M':
                number+=1000
        return number
```

## Explanation

* Set inital number to 0 and iterate over each numeral in input string.
* Create case for I, X, C so that if placed before respective numerals they subtrack instead of add, this is done by checking next index in string with enumeration index.  Use try so that if last element in list don't stop with IndexError and instead just add the numeral as a normal value.
* Add cases for other numerals based on table.
* Return final number.

## Result

Runtime: 48 ms, faster than 98.90% of Python3 online submissions for Roman to Integer.
Memory Usage: 13.1 MB, less than 90.79% of Python3 online submissions for Roman to Integer.
