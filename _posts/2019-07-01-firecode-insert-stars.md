---
title: "Firecode 10: Insert Stars"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Given a string, recursively compute a new string
where identical, adjacent characters
get separated with a *

Example:

```
insert_star_between_pairs("cac") ==> "cac"

insert_star_between_pairs("cc") ==> "c*c"
```



## My Code

```python
def insert_star_between_pairs(a_string):
    if a_string is None or len(a_string) == 1:
        return a_string
    if a_string[0] == a_string[1]:
        return (a_string[0] + "*" + insert_star_between_pairs(a_string[1:]))
    else:
        return (a_string[0] + insert_star_between_pairs(a_string[1:]))
```

## Explanation

* Doing it recursively so check if we only have one char remaining or none remaining in which case there's no need to do comparison to avoid index error.
* Compare first element in list (since recursive its the next one every time) to the next element to check if identical.  If so return first element + * + recursive call to func with next element in array to finish
* Else return first element and recursive call to rest of list.
