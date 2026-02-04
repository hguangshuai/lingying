# Python yield and Generators

## English Description / Topic
Explanation of the `yield` keyword in Python, how it creates generators, and its benefits for memory efficiency.

## Fundamental Knowledge

### 1. What is `yield`?
The `yield` keyword is used in a function like `return`, but instead of returning a value and exiting the function, it pauses the function's execution and "yields" a value to the caller. When the function is called again, it resumes right where it left off.

### 2. Generators
Functions containing `yield` are called **Generators**. They return an iterator object that produces a sequence of values on demand (lazy evaluation).

### 3. Key Benefits
- **Memory Efficiency**: Unlike lists, generators do not store all values in memory. They produce values one at a time. This is crucial for processing massive datasets or infinite sequences.
- **Lazy Evaluation**: Computation only happens when you request the next value.

## Spoken / Interview Answer
"In Python, `yield` is used to create **Generators**. Unlike a normal function that returns a single value and terminates, a function with `yield` can produce a sequence of values over time. 
The main advantage is **memory efficiency**. If you're processing a file with 10 million rows, using a list would crash your memory, but using `yield` allows you to stream one row at a time. It's essentially 'lazy evaluation' for sequences."

