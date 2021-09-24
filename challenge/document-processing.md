
# Challenge: Docment Processing

In this challenge, we will be working with a potentially **huge** sequence of documents where each document is a single string that is the entirety of the text of the document. Each document should easily fit into memory, but all of them at once won't!

This challenge is broken into several steps so that we can build the solution up piece by piece. The final solution will transform a sequence of documents into a sequence of words such that:
- each word has been normalized
- optionally, only unique words are emitted

## The Input
Below is a very short example input as a list, though the test cases will likely use some type of lazy iterable with the same structure.
```python
input = [
    "this is the text of the first document",
    "this is the text of the second document"
]
```
    
<!-- ----------------------- Part I ----------------------- -->
## Part I: Produce a sequence of words

Task: Given a sequence of documents, write a function that will extract and emit each word in the order it is encountered as a lazy sequence. The signature of the function is:
```python
def docsToWordsI(iterable documents) -> lazy_iterable words
```

Example Input/Outputs:
```python
Input1  : []
Output1 : []
Input2  : ["","","A"]
Output2 : ["A"]
Input3  : ["a Cat", "A caT", "a rAt"]
Output3 : ["a", "Cat", "A", "caT", "a", "rAt"]
```

### Solution:
<details>
<summary>Reveal</summary>

```python
input = [" A   caT  tExT   Dog Cat ", "rat SAT on  that sPlAT  ", "some rat text here dog gone"]

# transform input into a stream of words
def docsToWords(documents):
    for doc in documents:
        for word in doc.split():
            yield word

list(docsToWords(input))
```
</details>
<br><br>

<!-- ----------------------- Part II ----------------------- -->
## Part II: Clean up that input
Task: Update your solution from above to do some cleaning, or normalizing, of the input data. For normalizing, we would like to downcase all of the words, so that ["Be", "be", "BE"] all become ["be", "be", "be"]. The function signature for this step is:
```python
def docsToWordsII(iterable documents) -> lazy_iterable words
```

Example Input/Outputs:
```python
Input1  : []
Output1 : []
Input2  : ["","","A"]
Output2 : ["a"]
Input3  : ["a Cat", "A caT", "a rAt"]
Output3 : ["a", "cat", "a", "cat", "a", "rat"]
```

### Solution:
<details>
<summary>Reveal</summary>

```python
input = [" A   caT  tExT   Dog Cat ", "rat SAT on  that sPlAT  ", "some rat text here dog gone"]


def docsToWords(documents):
    for doc in documents:
        for word in doc.split():
            # here we are "normalizing"
            yield word.lower()

list(docsToWords(input))
```
</details>
<br><br>

<!-- ----------------------- Part III ----------------------- -->
## Part III: Wait, maybe uppercase instead, or camelcase...
Task: It seems that "normalize" means different things to different people, so let's update our solution so that we can pass in the normalize function and each user can define normalize however they like. The default should do no normalization. The updated signature is below with a sample invocation:

```python
def docsToWordsIII(iterable documents, function norm_f) -> lazy_iterable words

for each word in docsToWordsIII(input, lambda word: word.lower()):
    do_something(word)
```

Example Input/Outputs: if passed no normalization function the Input/Output are same as Part I, if passed this: ```lambda word: word.lower()``` as its normalization function the Input/Output will be the same as Part II.

### Solution:
<details>
<summary>Reveal</summary>

```python
# add the new parameter in the signature
def docsToWords(documents, norm_f=None):
    for doc in documents:
        for word in doc.split():
            # normalize if norm_f was provided
            yield norm_f(word) if norm_f else word

list(docsToWords(input, lambda word: word.lower()))
```
</details>
<br><br>

<!-- ----------------------- Part IV ----------------------- -->
## Part IV: Only the unique ones
Task: Finally, we would like to be able to get only unique words, so each word is emitted once when it is first encountered, but then never again. Let's enable this by adding a flag called unique that defaults to False. The final signature for our function is:
```python
def docsToWordsIII(iterable documents, function norm_f, boolean unique) -> lazy_iterable words
```

Example Input/Outputs: If unique is False, test Inputs/Outputs are the same as for part III. Below are test Inputs/Outputs if unique is True
```python
Input1  : []
Output1 : []
Input2  : ["","","A"]
Output2 : ["a"]
Input3  : ["a Cat", "A caT", "a rAt"]
Output3 : ["a", "cat", "rat"]
```

### Solution:
<details>
<summary>Reveal</summary>

```python
# add another new parameter
def docsToWords(documents, norm_f=None, unique=False):
    seen_words = {""} # this is a set
    for doc in documents:
        for word in doc.split():
            word_n = norm_f(word) if norm_f else word
            # logic to optionally produce only unique words
            if unique:
                if word_n not in seen_words:
                    seen_words.add(word_n)
                    yield word_n
            else:
                yield word_n

list(docsToWords(input, lambda word: word.lower()))
```
</details>