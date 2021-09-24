# Lecture Notes
- Topic : Python Generators: What they are and how to use them
- Target Time : 10 min (assuming questions after)

## Topics:
1. (Time: 3-4min) Explain and demonstrate what a generator is and why they are useful. 
2. (Time: 3-4min) Explain and demonstrate the two methods of creating a generator: 
    - with function syntax
    - with a generator-comprehension
3. (Time: 2-4min) Show some ways generators can be used. Pick from below or get creative 
    - multiple solutions to same problem
    - generators built from other generators
    - work with map, filter, reduce, zip
    - transformations on lists, sets, dictionaries
    - a file based example...

## Other Materials:
- The included article, [Python Generators: What, Why, How?](../article.md),
should be made available to the students such that they have time to read it
before the lecture.
- Programming Challenges
- Multiple Choice Questions
- Examples to pull from for the lecture
## Outline
The are the core details to hit:
- What is a generator?
    - A function that returns an iterator
    - Its iterator is lazily evaluated
    - has two faces: generator-function and generator-object
- Why are generators useful?
    - Iterators are used throughout python, so generators give coders
    an easy way to interact with powerful features of python.
    - Lazy evaluation allows us to work with sequences of things that won't all fit into memory at the same time
- Creating generators:
    - With function syntax and yield
    - With a generator-comprehension
- Using generators:
    - In a for-loop
    - In a continuation


