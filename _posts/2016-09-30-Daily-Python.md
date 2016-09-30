---
published: true
title: Daily Python Study
layout: post
author: jungwook
category: Language
tags:
- python
---

Refer from Enki : <https://www.enki.com/>

### **Set Comprehension**

Using **set comprehension** one can create sets using the same principles as with **list comprehension**.

They syntax difference between the two is that set comprehension requires using *curly* brackets({}).

Imagine we have the following list:

```{.python}
my_list = [ 1, 2, 3, 4, 5, 6, 7, 8]
```

And we need s set containing only even numbers in the list. This can be easily achieved with **set comprehension**

```{.python}
even_set = { x for in my_list if x%2 == 0 }
```

We can now check the result:

```{.python}
print(even_set)
# {8, 2, 4, 6}
```

Note that the above operation would work even if my_list contained some duplicate values, e.g:

```{.python}
my_list = [1, 2, 3, 4, 5, 6, 7, 8, \
8, 7, 6
```

### **Neasted lists comprehension**

Since a list comprehension can take any **expression** as its initial expression, it\`s possible to include another list comprehension at that position, resulting in a **neasted list** comprehension.

These are often useful, but are often used to work with matrices.

```{.python}
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

Say we went to create another matrix with values equal to the squares of each element in the original matrix:

```{.python}
matrix2 = [[x**2 for x in row] for row in matrix]
#matrix2 = [[1, 4, 9], [16, 25, 36], [49, 64, 81]]
```

A more advanced list comprehension with two for clauses and two if clauses:

```{.python}
```

A more advanced list comprehension with two for clauses and two if clauses:

```{.python}
lc = [ ( x, y ) for x in range(10) if x % 2 == 0 for y in range(20) if y %3 == 0 ]
#[(0, 0), (0, 3), (0, 6), (0, 9), (0, 12), (0, 15), (0, 18), (2, 0), (2, 3), (2, 6), (2, 9), (2, 12), (2, 15), (2, 18), (4, 0), (4, 3), (4, 6), (4, 9), (4, 12), (4, 15), (4, 18), (6, 0), (6, 3), (6, 6), (6, 9), (6, 12), (6, 15), (6, 18), (8, 0), (8, 3), (8, 6), (8, 9), (8, 12), (8, 15), (8, 18)]
```

### **Dictionary comprehension**

Python provides a way of creating dictionaries using **dictionary comprehension**

The syntax is similar to the one used for **set comprehension** - using curly braces({}). but the user must define an expression for both the key and the value.

Take that list :

```{.python}
num_list = [1, 2, 3, 4, 5]
```

To get a dict with its keys equal to the numbers in the list, and the corresponding values being the cubes of the keys:

```{.python}
cube_dict = {x: x ** 3 for x in num_list }
```

Now if we print cube_dict.items():

```{.python}
for k, v in cube_dict.items():
    print(k, v)
# output
# 1 1
# 2 8
# 3 27
# 4 64
# 5 125
```

If we want to initialize the counters to zero in a dict to count frequencies of lower case letters in some input:

```{.python}
lcase_freq = { chr(c): 0 for c in range(ord('a'), ord('z') + 1)}

print(lcase_freq)
# partial output ...
{'l': 0, 'n': 0, 'f': 0, 'h': 0, 'r': 0, 'k': 0, 'd': 0, 'y': 0, 'u': 0, 's': 0, 'z': 0, 'i': 0, 'p': 0, 't': 0, 'm': 0, 'q': 0, 'a': 0, 'c': 0, 'b': 0, 'x': 0, 'o': 0, 'g': 0, 'j': 0, 'w': 0, 'v': 0, 'e': 0}

# Check it is correct:
lfk = list(lcase_freq.keys())
lfk.sort()
print(lfk)
['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
```

### **List comprehension**

**List comprehensions** are a concise way of creating lists, in a somewhat declarative style.

One common application is to create new lists whose items are the result of operations applied to other sequences or iterables, or in other words, functions of other iterables ( item for item ).

The syntax for this is to include between **square brackets**([]) an expression followed by at least one `for` clause.

Create a list of squares:

```{.python}
squares = [x ** 2 for x in range(6)]
print(squares)
# output
[0, 1, 4, 9, 16, 25]
```

Expressions that consist of `tuples` must be parenthesized:

```{.python}
squares = [(x, x ** 2) for x in range(4)]
print(squares)
# output
[(0, 0), (1, 1), (2, 4), (3, 9)]
```

Note that `if` clauses are also accepted:
```{.python}
a = [ x ** 2 for x in range(20) if x % 5 == 0]
# output
[0, 25, 100, 225]

b = [ i for i in range(10) if i * i < 50 ]
# output
[0, 1, 2, 3, 4, 5, 6, 7]
```

List comprehensions can contain complex expressions and nested functions:

```{.python}
from math import pi

b = [ str(round(pi, i)) for i in range(1, 5)]
# output
['3.1', '3.14', '3.142', '3.1416']
```

What makes it more powerful is that even the if clause can contain calls to functions that become part of the overall boolean expression. Simple example:

```{.bash}
def square(n):
    return n*n

print(square(5))
# 25
print(square(6))
# 36
a = [ i for i in range(10) if square(i) < 50 ]
print(a)
#[0, 1, 2, 3, 4, 5, 6, 7]
```

### **Speed up your for loop using map() or list comprehensions**

The `for` loop interpreter overhead can be huge. So here `map()` functions like a `for` loop in C code.

Example:

If you need to lowercase all the input strings:

```{.python}
lowerList = []

for word in inputList:
    lowerList.append(word.lower())
```

Instead, you can use `map()` to push the loop into compiled C code:

```{.python}
lowerList = map(str.lower, inputList)
```

Also, python 2.0 or above, there are list comprehensions:

```{.python}
lowerList = [word.lower() for word in inputList]
```

They are both more efficient than simple `for` loop statement

