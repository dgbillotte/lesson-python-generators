
Question Which of the following comprehension statements will result in a generator?
1. ```[ item for item in items if is_prime(item) ]```
2. ```{ item for item in items if is_prime(item) }```
3. ```( item for item in items if is_prime(item) )```
4. ```([ item for item in items if is_prime(item) ])```

Question Bonus: the generator above is a:
1. generator function
2. generator object

Question What is the result of running the following code?
```python
def roulette(start):
    countdown = start
    while True:
        decrement = 1
        if countdown % 5 == 3:
            decrement = 4
            
        elif countdown % 3 == 2:
            decrement = 5

        if countdown == 0:
            print("hurray, you dodged it")
            return
            
        if countdown < 0:
            raise Exception("You got blowed up")
            return
        
        yield countdown
        countdown -= decrement
        
list(roulette(17))
```
1. Compiler error stating that return cannot be used inside a generator
2. Exception: "You got blowed up"
3. "hurray..." with [17, 12, 11, 6, 5]
4. "hurray..." with [17, 12, 10, 6, 5]


Question Given the following two generator definitions:
```python
g1 = (i for i in range(10) if i % 3 == 0)

def g2():
    for i in range(10):
        if i % 3 ==0:
            yield i
```
which usage in a for-loop will cause an error?
1. ```for i in g1:```
2. ```for i in g1.__iter__():```
3. ```for i in g2:```
4. ```for i in in g2():```
5. none of the above


Question Given this generator definition and invocation
```python
def odds():
    n = 1
    while True:
        yield n
        n += 2
        
mylist = list(odds())
```
After the code is run, mylist contains which?
1. A generator for all of the positive odd integers
2. A list of all of the positive odd integers
3. An empty list
4. Unknown because it would take infinite memory

Question Bonus: the generator above, odds, is a:
1. generator function
2. generator object

Question Given this code:
```python
a = [i for i in [1,2,3]]
b = (i for i in [1,2,3])
```
We can show that
```python
list(a) == list(b)
```
We can therefore assert that ```a == b```.
1. True
2. False
