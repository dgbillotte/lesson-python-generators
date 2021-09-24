# Lecture Notes
## Overview
### Topic : Python Generators: What they are and how to use them
### Target Time : 10 min (assuming questions after)
<br>

## Topics with Estimated Time:
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
    
Note: spend only as much time as needed on parts 1 & 2 and try to maximize part 3.

## Other Materials:
- The included article, [Python Generators: What, Why, How?](../article.md),
should be made available to the students such that they have time to read it
before the lecture.
- Programming Challenges:
    - [Exercises](../challenge/exercises.md) : A collection of stand-alone exercises.
    - [Document Processing](../challenge/document-processing.md) : A 4 part challenge that incrementally builds up a tool to process the text of a sequence of documents.
- [Multiple Choice Questions]() : Can be used for quizes or tests.
- For the lecture, use exercises from [challenge](../challenge) that don't get used for assignments or feel free to add more examples there.

## Outline
These are the core details to hit:
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


