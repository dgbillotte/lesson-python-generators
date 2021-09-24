# Exercises
This is a collection of stand-alone exercise to help you gain familiarity and experience with generators in python.


<!-- ----------------------- Exercise 1 ----------------------- -->
## Exercise 1: dict.items()
The ```dict.items()``` method will return all of the key/value pairs in a dictionary as a list of pairs. Take a minute to familiarize yourself with it.

Using a generator function, create a generator called ```items1()``` which will take a dictionary and return a sequence of key/value pairs, **without** using the built-in method.

The signature for the function is:
```python
def items1(dictionary dict) -> lazy_iterable pairs
```

Example Input/Outputs:
```python
Input1  : {}
Output1 : []
Input2  : { 'a': 1 }
Output2 : [('a', 1)]
Input3  : { 'a': 1, 'b': 2 }
Output3 : [('a', 1), ('b', 2)]
```

### Solution:
<details>
<summary>Reveal</summary>

```python
def items1(dict):
    for key in dict.keys():
        yield (key, dict[key])

input = { 'a': 1, 'b': 2 }
list(items1(input))
```
</details>
<br><br>

<!-- ----------------------- Exercise 2 ----------------------- -->
## Exercise 2: dict.items(), as a comprehension
Do exercise #1, but use a comprehension instead of a generator function.
Hint: your solution will have the same interface as Exercise #1 except
the function will be called ```items2()```

### Solution:
<details>
<summary>Reveal</summary>

```python
def items2(dict):
    return ((key, dict[key]) for key in dict.keys())

input = { 'a': 1, 'b': 2 }
list(items2(input))
```
</details>
<br><br>

<!-- ----------------------- Exercise 3 ----------------------- -->
## Exercise 3 : Collapsing dimensions
In this exercise you will create a function, ```collapse()``` that will take a 2d list or a "sequence of 1d lists", however you'd like to look at it, and collapse it down to a 1d sequence by replacing each internal list with the average of its values. Below is a walk through of an invocation of it:
```python
Input: [[1,2],[3,4,5],[6]]
    -> [avg([1,2]), avg([3,4,5]), avg([6])]
    -> [1.5, 4.0, 6.0]
```

Hint: the ```functools.reduce()``` function may be useful here...

The signature for the function is:
```python
def collapse(iterable twoD) -> lazy_iterable averages
```


Example Input/Outputs:
```python
Input1  : []
Output1 : []
Input2  : [[]]
Output2 : TypeError exception is expected
Input3  : [[4]]
Output3 : [4.0]
Input4  : [[1,2],[3,4,5],[6]]
Output4 : [1.5, 4.0, 6.0]
```

### Solution:
<details>
<summary>Reveal</summary>

```python
from functools import reduce

def collapse(twoD):
    return (reduce(lambda val, sum: val+sum, sublist)/len(sublist)
            for sublist in twoD)

twoD = [[2,3,4],[3,8],[7],[6,4,2,1]]
list(collapse(twoD))
```
</details>
<br><br>

<!-- ----------------------- Exercise 4 ----------------------- -->
## Exercise 4: Find the running maximum consecutive pair
Create a function that takes an input sequence of numbers and tracks (and outputs) the largest pair of consecutive items as it processes the stream. Below is a walk through of an invocation of the function:
```python
list(max_siblings([1,3,2,9,4,3,1]))
    -> (1,3) # 1+3 > max
    -> (3,2) # 3+2 > max
    -> (2,9) # 2+9 > max
    -> (9,4) # 9+4 > max
    -> # no output 4+3 < 13
    -> # no output 3+1 < 13
    -> [(1,3),(3,2),(2,9),(9,4)] 
```

Note that a pair is emitted *only* when a new maximum is encountered.

The signature for the function is:
```python
def max_siblings(iterable list) -> lazy_iterable current max pair
```

Example Input/Outputs:
```python
Input1  : []
Output1 : []
Input2  : [1]
Output2 : [] # !! we don't have any pairs yet...
Input3  : [1,2]
Output3 : [(1,2)]
Input4  : [1,3,2,9,4,3,1]
Output4 : [(1, 3), (3, 2), (2, 9), (9, 4)]
```

### Solution:
<details>
<summary>Reveal</summary>

```python
def max_siblings(list):
    max = None
    last = 0
    for item in list:
        if (max == None):
            max = (last, item)
        elif(last + item > max[0] + max[1]):
            max = (last, item)
            yield max
            
        last = item
test = [1,7,2,3,5,7,3,8,3,5,7,3,8,4]
list(max_siblings(test))
```
</details>