---
title: "Algoexpert: Caesar Cipher Encryptor"
categories:
  - Algoexpert
tags:
  - algoexpert
  - python
---
## My Code

```python
def caesarCipherEncryptor(string, key):
	# #Write your code here.
	# #26 letters in alphabet
	# key = key % 26
	# new_string = []
	# for letter in string:
	# 	new_letter_uni = ord(letter) + key
	# 	#alphabet unicode/ord = 97(a) -> 122(z)
	# 	if new_letter_uni <= 122:
	# 		new_string.append(chr(new_letter_uni))
	# 	else:
	# 		#96 because 1 below a ord value, 122 mod because see how many number over z it needs to be shifted
	# 		new_string.append(chr(96 + (new_letter_uni%122)))
	# return "".join(new_string)

	alphabet = "abcdefghijklmnopqrstuvwxyz"
	alphList = list(alphabet)
	new_string = []
	for letter in string:
		index = alphList.index(letter)
		new_key = key % 26
		shifted_index = index + new_key
		if shifted_index <= 25:
			new_string.append(alphList[shifted_index])
		else:
			new_string.append(alphList[(-1+(shifted_index%25))])
	return "".join(new_string)
```

## Explanation

* Method 1:
* Uses unicode conversion with python ord() and chr() in order to convert the letter to a unicode representation then shift.
* First solve edge case of shift/key value being more than a full shift through alphabet, e.g. > 26, by doing key %26 so it goes to the same value but only once through.
* For every letter in the string, get the ordinal value of the letter summed with the key/shift value.
* If the value is <=122(ord value of z) we can append the chr() representation of the value to the new string list.
* Otherwise, calculate the amount it's over 122(value%122) and add that to 96(97 equates to a, so if its 1 over 122 then it'll add 1 to 96 giving A) and append chr representation
* Return joined string from list

* Method 2:
* Uses an inital alphabet list instead of unicode conversions.
* convert alphabet string to list.
* For every letter in the list, find the index of it in the alphabet list from earlier(alphList.index(element)) and add that to the key for the shifted index value.
* IF the shifted index value <=25(alphabet list is 0-25 indexes for 26 chars) can append the alphabet value at the index point.
* Otherwise find the %25 value for how many over 25 it is and add to -1(list starts at 0).
* Return joined list
