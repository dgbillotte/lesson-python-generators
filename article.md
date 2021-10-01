# Python Generators: What, Why, How?

Today we are going to discuss the creation and use of generators in python. We will first examine what a generator is and why they are useful. Next we will look at two different ways to create generators. Finally, we will wrap up with a problem and several different ways that we can solve the problem with generators.

## What is a Generator?
A generator is a function that when called, returns an iterator. This may sound simple at first, but because of the way that python uses iterators in core language constructs, like for-loops and comprehensions, and all of its core collection-types (lists, dictionaries, sets, tuples??), having an easy way to create custom iterators allows one to use python to its fullest. Let's look at a familiar example, the for-loop:

```python
# print 1 to 3, each on its own line
for i in [1,2,3]:
    print(i)

# print 1 to 10, each on its own line
for i in range(1,11):
    print(i)

```
In both examples above, we are handing the for-loop either an iterator or an iterable. Iterators work equally seemlessly with comprehensions as well as the map, filter, reduce, and zip functions. Being able to easily create your own iterators allows you write code that leverages these powerful features of python.

## Creating Generators
There are two ways to create a generator. We will look at the function-like way first so that we can understand how generators work and then we will see how we can create some types of generators directly with comprehensions.

### The Function-like Way
Here are two simple generators:

```python
# generator that will emit 1,2,3
def g123():
    for n in [1,2,3]:
        yield n

# generator that will emit the contents of a list
def glist(list):
    for n in list:
        yield n


# these can be used just like range()
for i in g123():
    print i

list = [4,5,6]
for n in glist(list):
    print n
```
The only difference from a "normal" function definition is the use of the keyword ```yield``` instead of ```return```.

```yield``` acts much like ```return``` in that it "returns" a value back to a calling scope. There are two primary differences. First, the ```yield``` keyword tells the python interpreter that this function is a generator and to compile it as such. Second, when ```yield``` is called, it signals to the interpreter that it should maintain the state/scope of the generator so that it can be called again. If you call return inside a generator or let the code run to the end of the function it will terminate the generator and release its scope.

----

Before digging deeper it is important to make the [distinction][1] between a "generator function" and a "generator object". When used in the context of a for-loop, these details can get blurred a little bit. The definitions of ```g123()``` and ```glist()``` are generator functions. In ipython we can see that they are just functions:
```python
In [96]: def g():
    ...:     for n in [1,2,3]:
    ...:         yield n

In [97]: type(g)
Out[97]: function
```

What is more interesting is the generator object that is returned when the function is called:
```python
In [98]: type(g())
Out[98]: generator
```
Using the ```inspect.getmembers()``` function we can see that the generator has both ```__iter__()``` and ```__next__()``` methods on it, telling us that it is an iterator.

```python
In [99]: getmembers(g())
Out[99]: 
[('__class__', generator),
    ...
 ('__iter__', <method-wrapper '__iter__' of generator object at 0x1094072e0>),
    ...
 ('__next__', <method-wrapper '__next__' of generator object at 0x1094072e0>),
    ...
```

Below are some more example generators that get a little more interesting. There are two things that I would like to point out as you study them:
- The iterator doesn't need to be based off of an "in memory" sequence, such as a list, like our previous generators. The items can be created as needed and only as needed as seen in ```range()``` and ```upto_inf()```. This is called a "lazy sequence" and can be very useful for dealing with sequences that are very large, have very large items, or have items which don't yet exist...
- Generators can be easily composed from other generators.


```python
# simplified implementation of range()
def range(start, stop, step=1):
    next = start
    while next < stop:
        yield next
        next += step


# create an infinite sequence
def upto_inf(start=0, max=None, step=1):
    next = start
    while max == None or next < max:
        yield next
        next += step


# infinite list of squares
def squares_to_inf(start=0, max=None):
    for n in upto_inf(start, max):
        yield n*n


# another implementation of range() that builds off
# of upto_inf(), but protects against infinite sequences
def range2(start, stop, step=1):
    # no infinite ranges allowed
    if type(stop) not in (int, float):
        raise TypeError("range must have a numeric stop value")

    for n in upto_inf(start, stop, step):
        yield n
```

### Creating Generators with a Comprehension
Now that we really know what a generator is and how to create them as generator functions, let's see how we can create them with a comprehension:

```python
# create a list of squares of 1-5
sqs1 = [ n*n for n in range(1, 6) ]

# create a generator of the squares of 1-5
sqs2 = ( n*n for n in range(1, 6) )
```
The only difference, above, is the use of parenthesis, "()", instead of brackets, "[]", in the comprehension definition. They both are iterable, so what is the difference?

sqs2 is lazy, *very lazy*. You have to tell it to do every step, which is great because it will never do any steps that you don't ask it to do, and won't occupy memory for each step until it absolutely has to. We can see that by looking at the memory used for each object:

```python
In [21]: getsizeof( [ n*n for n in range(1, 100) ] )
Out[21]: 920 # this is a list

In [22]: getsizeof( ( n*n for n in range(1, 100) ) )
Out[22]: 112 # this is a generator
```
Note: one little trick for working with lazy sequences, since they are lazy, you can't just print them out with ```print()``` because the items might not exist yet. To inspect the contents of a lazy sequence you have to force it to be evaluated. A simple way to do this is with the ```list()``` constructor, like this:
```python
# the contents of the generator are not visible
In [45]: (i for i in range(10))
Out[45]: <generator object <genexpr> at 0x10b7e93c0>

# casting to a list forces the evaluation of the generator contents
In [46]: list( (i for i in range(10)) )
Out[46]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# this might not work out very well
In [47]: list(upto_inf())
Out[47]: ... "out to lunch. be back never"
```

Regarding the two different generator creation mechanisms, there is one difference between the two. Generators defined as functions must be invoked to create the generator object, while a generator that is defined as a comprehension is a generator object and can invoked directly. Here is an example with the generators defined above:
```python
# using our range() generator
for i in range(1,10): # we are invoking the generator function
    print i

# using or sqs2 generator
for i in sqs2: # sqs2 is a generator object
    print i
```

Now that we understand generators and how to create them it is time to look at more ways that we can use them with python's built in tools.

## Generators in Action
Here we will look at several different ways that we can solve a problem with generators.

Problem: create a function, pairup(sequence), that will transform its input list into a sequence of pairs of numbers that were contiguous in the input list. Here's an example:

- input: [1,2,3,4]
- output: [(1,2), (2,3), (3,4)]

Let's first create a generator function using a for-loop:
```python
# generator to pair up neighbor elements in a list
def pairup(seq):
    for v in range(len(seq) - 1):
        yield (seq[i], seq[i+1])

# use the generator in a loop
for p in pairup(range(10)):
    print(p[0], " and ",  p[1], "are neighbors")

# same but with destructuring
for a,b in pairup(range(10)):
    print(a, " and ",  b, "are neighbors")


```

Let's do the same using ```zip()``` and a naked comprehension:
```python
# using a naked comprehension
seq = range(10)
generator = ( pair for pair in zip(seq[:-1], seq[1:]) )
for p in generator:
    print(p[0], " and ",  p[1], "are neighbors")
```

Now let's wrap that in a function so that the input sequence can be easily passed in instead of being bound when the continuation is created. Interestingly, now it looks and acts just like a generator function:
```python
# wrapped up all nice and tidy like a generator function
def pairup2(seq):
    return ( pair for pair in zip(seq[:-1], seq[1:]) )

for p in pairup2(range(10)):
    print(p[0], " and ",  p[1], "are neighbors")
```

## Conclusion
In this article we:
- Described what a generator is and why they are useful
- How to create a generator with either: a generator-function or a generator-comprehension
- How to compose generators from other generators
- How to use generators in for-loops and in comprehensions

## References 
- ["Generators in Python" by Shwetanshu Rohatgi][1]
- [python.org docs][2]

[1]: <https://www.geeksforgeeks.org/generators-in-python/> "Generators in Python by Shwetanshu Rohatgi"
[2]: <https://www.python.org/doc/> "python.org docs"
