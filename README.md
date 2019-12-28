# Iterator Algorithms

A collection of iterator algorithms for Python3 inspired by the algorithms library of C++.

## Author: Robert Sharp

### Quick Install:
```shell script
$ python3 -m pip install IteratorAlgorithms
```

### Run Basic Test Suite:
```shell script
$ python3 -m IteratorAlgorithms
... # Test Output
Test passed.
$ 
```
Tests are verbose by default. Tests are only run when the module is executed as a script, as above.

### Standard Import:
```shell script
$ python3
>>> import IteratorAlgorithms
# No Test Output. Ready for work!
>>>
```
None of the standard import styles should trigger the tests.

### Help Features
All of the features of this module have full help support built in.
```pydocstring
$ python3
>>> from IteratorAlgorithms import fork
>>> help(fork)
Help on function fork in module __main__:

fork(array: Iterable, forks: int = 2) -> tuple
    Fork
    Iterator Duplicator.
    
    # DocTest:
    >>> it = iter(range(10))
    >>> a, b, c = fork(it, 3)
    >>> list(c)
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    >>> a == b
    False
    >>> list(a) == list(b)
    True
    
    @param array: Iterable to be forked.
    @param forks: Optional Integer. Default is 2. Represents the number of forks.
    @return: tuple of N Iterators where N is the number of forks.
```

## Table of Contents:
- Generators
    - iota
    - generate
    - generate_n
- Expansions
    - fork
    - exclusive_scan
    - inclusive_scan
- Transforms
    - transform
    - adjacent_difference
    - partial_sum
- Permutations
    - partition
- Reductions
    - reduce
    - accumulate
    - product
- Queries
    - all_of
    - any_of
    - none_of
- Transform & Reduction
    - transform_reduce
    - inner_product
- Multidimensional Reductions
    - zip_transform
    - transposed_sums
- Multi-Set Operations
    - union
    - intersection
    - difference
    - symmetric_difference

---


## Generators

### Iota
```pydocstring
Help on function iota in module IteratorAlgorithms:

iota(start, stop=None, step=1, stride=1) -> Iterator
    Iota
    Iterator of a given range with stride grouping.
    
    DocTests:
    >>> list(iota(11))
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    >>> list(iota(start=1, stop=11))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    >>> list(iota(start=2, stop=21, step=2))
    [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
    >>> list(iota(start=2, stop=21, step=2, stride=2))
    [(2, 4), (6, 8), (10, 12), (14, 16), (18, 20)]
    
    @param start: Beginning
    @param stop: Ending
    @param step: Stepping
    @param stride: Number of groups.
    @return: Iterator of a given multidimensional range.

```
### Generate
```pydocstring
Help on function generate in module IteratorAlgorithms:

generate(transformer: Callable, *args, **kwargs)
    Generate
    Abstract generator function. Infinite Iterator.
    
    DocTests:
    >>> counter = itertools.count(1)
    >>> gen = generate(next, counter)
    >>> list(next(gen) for _ in range(10))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    
    @param transformer: Functor.
    @param args: Positional arguments for the functor.
    @param kwargs: Keyword arguments for the functor.

```
### Generate_N
```pydocstring
Help on function generate_n in module IteratorAlgorithms:

generate_n(n: int, transformer: Callable, *args, **kwargs)
    Generate N
    Abstract generator function. Finite.
    
    DocTests:
    >>> counter = itertools.count(1)
    >>> list(generate_n(10, next, counter))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    
    @param n: Maximum number of elements.
    @param transformer: Functor.
    @param args: Positional arguments for the functor.
    @param kwargs: Keyword arguments for the functor.

```

## Expansions

### Fork
```pydocstring
Help on function fork in module IteratorAlgorithms:

fork(array: Iterable, forks: int = 2) -> tuple
    Fork
    Iterator Duplicator.
    
    DocTests:
    >>> it = iter(range(10))
    >>> a, b, c = fork(it, 3)
    >>> list(c)
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    >>> a == b
    False
    >>> list(a) == list(b)
    True
    
    @param array: Iterable to be forked.
    @param forks: Optional Integer. Default is 2. Represents the number of forks.
    @return: Tuple of N Iterators where N is the number of forks.

```
### Exclusive_Scan
```pydocstring
Help on function exclusive_scan in module IteratorAlgorithms:

exclusive_scan(array: Iterable, init) -> Iterator
    Exclusive Scan Pairs
    Inserts an initial value at the beginning and ignores the last value.
    
    DocTests:
    >>> list(exclusive_scan(range(1, 10), 0))
    [(0, 1), (1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8)]
    >>> list(exclusive_scan(range(1, 10), 10))
    [(10, 1), (1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8)]
    
    @param array: Iterable to be scanned.
    @param init: Initial Value.
    @return: Iterator of Pairs.

```
### Inclusive_Scan
```pydocstring
Help on function inclusive_scan in module IteratorAlgorithms:

inclusive_scan(array: Iterable) -> Iterator
    Inclusive Scan Pairs
    
    DocTests:
    >>> list(inclusive_scan(range(1, 6)))
    [(1, 2), (2, 3), (3, 4), (4, 5)]
    >>> list(inclusive_scan(range(1, 10)))
    [(1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8), (8, 9)]
    
    @param array: Iterable to be scanned.
    @return: Iterator of Pairs.

```

## Transforms

### Transform
```pydocstring
Help on function transform in module IteratorAlgorithms:

transform(array: Iterable, func: Callable) -> Iterator
    Transform
    Similar to map but with a reversed signature.
    
    DocTests:
    >>> list(transform(range(1, 10), add_one))
    [2, 3, 4, 5, 6, 7, 8, 9, 10]
    >>> list(transform(range(1, 10), square))
    [1, 4, 9, 16, 25, 36, 49, 64, 81]
    
    @param array: Iterable of Values.
    @param func: Unary Functor. F(x) -> Value
    @return: Iterator of transformed Values.

```
### Adjacent_Difference
```pydocstring
Help on function adjacent_difference in module IteratorAlgorithms:

adjacent_difference(array: Iterable) -> Iterator
    Adjacent Difference
    Calculates the difference between adjacent pairs.
    This is the opposite of Partial Sum.
    The first iteration compares with zero for proper offset.
    
    DocTests:
    >>> list(adjacent_difference(range(1, 10)))
    [1, 1, 1, 1, 1, 1, 1, 1, 1]
    >>> list(adjacent_difference(partial_sum(range(1, 10))))
    [1, 2, 3, 4, 5, 6, 7, 8, 9]
    
    @param array: Iterable of Numeric Values.
    @return: Iterable of adjacent differences.

```
### Partial_Sum
```pydocstring
Help on function partial_sum in module IteratorAlgorithms:

partial_sum(array: Iterable) -> Iterator
    Partial Sum
    Calculates the sum of adjacent pairs.
    This is the opposite of Adjacent Difference.
    
    DocTests:
    >>> list(partial_sum(range(1, 10)))
    [1, 3, 6, 10, 15, 21, 28, 36, 45]
    >>> list(partial_sum([1, 1, 1, 1, 1, 1, 1, 1, 1, 1]))
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    
    @param array: Iterable of Numeric Values.
    @return: Iterator of adjacent sums.

```

## Permutations

### Partition
```pydocstring
Help on function partition in module IteratorAlgorithms:

partition(array: Iterable, predicate: Callable[[Any], bool]) -> Iterator
    Stable Partition
    
    DocTests:
    >>> list(partition(range(1, 10), is_even))
    [2, 4, 6, 8, 1, 3, 5, 7, 9]
    >>> list(partition(range(1, 10), is_odd))
    [1, 3, 5, 7, 9, 2, 4, 6, 8]
    
    @param array: Iterable of values to be partitioned.
    @param predicate: Unary functor. F(element) -> bool
    @return: Partitioned Iterator.

```

## Reductions

### Reduce
```pydocstring
Help on function reduce in module IteratorAlgorithms:

reduce(array: Iterable, func: Callable, initial=None)
    Reduce
    Similar to accumulate but allows any binary functor and/or an initial value.
    
    DocTests:
    >>> reduce(range(1, 5), operator.add)
    10
    >>> reduce(range(1, 5), operator.mul)
    24
    
    @param array: Iterable of Values to be reduced.
    @param func: Binary Functor.
    @param initial: Initial value. Typically 0 for add or 1 for multiply.
    @return: Reduced Value.

```
### Accumulate
```pydocstring
Help on function accumulate in module IteratorAlgorithms:

accumulate(array: Iterable)
    Accumulate
    Sums up a range of elements. Same as Reduce with operator.add
    
    DocTests:
    >>> accumulate(range(5))
    10
    >>> accumulate(range(11))
    55
    
    @param array: Iterable of Values to be summed.
    @return: Sum of Values.

```
### Product
```pydocstring
Help on function product in module IteratorAlgorithms:

product(array: Iterable)
    Product
    Reduce with multiply.
    For counting numbers from 1 to N: returns the factorial of N.
    
    DocTests:
    >>> product(range(1, 5))
    24
    >>> product(range(5, 10))
    15120
    
    @param array: Iterable of Values to be reduced.
    @return: Product of all elements multiplied together.

```

## Queries

### All_Of
```pydocstring
Help on function all_of in module IteratorAlgorithms:

all_of(array: Iterable, predicate: Callable) -> bool
    All of These
    
    DocTests:
    >>> all_of([], is_even)
    True
    >>> all_of([2, 4, 6], is_even)
    True
    >>> all_of([1, 4, 6], is_even)
    False
    >>> all_of([1, 3, 5], is_even)
    False
    
    @param array: Iterable to inspect.
    @param predicate: Callable. f(x) -> bool
    @return: Boolean.

```
### Any_Of
```pydocstring
Help on function any_of in module IteratorAlgorithms:

any_of(array: Iterable, predicate: Callable) -> bool
    Any of These
    
    DocTests:
    >>> any_of([], is_even)
    False
    >>> any_of([2, 4, 6], is_even)
    True
    >>> any_of([1, 4, 6], is_even)
    True
    >>> any_of([1, 3, 5], is_even)
    False
    
    @param array: Iterable to inspect.
    @param predicate: Callable. f(x) -> bool
    @return: Boolean.

```
### None_Of
```pydocstring
Help on function none_of in module IteratorAlgorithms:

none_of(array: Iterable, predicate: Callable) -> bool
    None Of These
    
    DocTests:
    >>> none_of([], is_even)
    True
    >>> none_of([2, 4, 6], is_even)
    False
    >>> none_of([1, 4, 6], is_even)
    False
    >>> none_of([1, 3, 5], is_even)
    True
    
    @param array: Iterable to inspect.
    @param predicate: Callable. f(x) -> bool
    @return: Boolean.

```

## Transform & Reduce

### Transform_Reduce
```pydocstring
Help on function transform_reduce in module IteratorAlgorithms:

transform_reduce(lhs: Iterable, rhs: Iterable, transformer: Callable, reducer: Callable)
    Transform Reduce
    Pairwise transform and reduction.
    
    DocTests:
    >>> transform_reduce(range(1, 6), range(1, 6), operator.mul, sum)
    55
    >>> transform_reduce(range(1, 6), range(1, 6), operator.add, product)
    3840
    
    @param lhs: Left Iterator
    @param rhs: Right Iterator
    @param transformer: Binary Functor F(x, y) -> Value
    @param reducer: Reduction Functor F(Iterable) -> Value
    @return: Reduced Value

```
### Inner_Product
```pydocstring
Help on function inner_product in module IteratorAlgorithms:

inner_product(lhs: Iterable, rhs: Iterable)
    Inner Product
    Preforms pairwise multiplication across the iterables,
        then returns the sum of the products.
    
    DocTests:
    >>> inner_product(range(1, 6), range(1, 6))
    55
    >>> inner_product(range(11), range(11))
    385
    
    @param lhs: Left Iterator
    @param rhs: Right Iterator
    @return: Sum of the products.

```

## Multidimensional Reductions

### Zip_Transform
```pydocstring
Help on function zip_transform in module IteratorAlgorithms:

zip_transform(transformer: Callable, *args: Iterable) -> Iterator
    Zip Transform
    The transformer should take the same number of arguments as there are iterators.
    Each iteration will call the transformer on the ith elements.
        F(a[i], b[i], c[i]...) ... for each i.
    
    DocTests:
    >>> l1 = (0, 1, 2, 3)
    >>> l2 = (8, 7, 6, 5)
    >>> l3 = (1, 1, 1, 1)
    >>> list(zip_transform(add_all, l1, l2, l3))
    [9, 9, 9, 9]
    
    @param transformer: Functor: F(*args) -> Value
    @param args: Any number of Iterators.
    @return: Iterator of transformed Values.

```
### Transposed_Sums
```pydocstring
Help on function transposed_sums in module IteratorAlgorithms:

transposed_sums(*args: Iterable) -> Iterator
    Transposed Sums - Column Sums
    The size of the output iterator will be the same as
        the smallest input iterator.
    
    DocTests:
    >>> l1 = (0, 1, 2, 3)
    >>> l2 = (8, 7, 6, 5)
    >>> l3 = (1, 1, 1, 1)
    >>> list(transposed_sums(l1, l2, l3))
    [9, 9, 9, 9]
    
    @param args: Arbitrary number of Iterators of numeric values.
    @return: Iterator of transposed sums aka column sums.

```

## Multi-Set Operations

### Union
```pydocstring
Help on function union in module IteratorAlgorithms:

union(*args: set) -> set
    Multiple Set Union
    Includes all elements of every set passed in.
    
    DocTests:
    >>> s1 = {0, 2, 4, 6, 8}
    >>> s2 = {1, 2, 3, 4, 5}
    >>> s3 = {2, 8, 9, 1, 7}
    >>> union(s1, s2, s3)
    {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    
    @param args: Arbitrary number of sets.
    @return: Unified set

```
### Intersection
```pydocstring
Help on function intersection in module IteratorAlgorithms:

intersection(*args: set) -> set
    Multiple Set Intersection
    Includes all elements that are common to every set passed in.
    If there is no intersection, it will return the empty set.
    If all sets are the same, it will return the union of all sets.
    Opposite of symmetric_difference.
    
    DocTests:
    >>> s1 = {0, 2, 4, 6, 8}
    >>> s2 = {1, 2, 3, 4, 5}
    >>> s3 = {2, 8, 9, 1, 7}
    >>> intersection(s1, s2, s3)
    {2}
    
    @param args: Arbitrary number of sets.
    @return: Set of common elements

```
### Difference
```pydocstring
Help on function difference in module IteratorAlgorithms:

difference(*args: set) -> set
    Multiple Set Difference
    Includes every element in the first set that isn't in one of the others.
    If there is no difference, it will return the empty set.
    
    DocTests:
    >>> s1 = {0, 2, 4, 6, 8}
    >>> s2 = {1, 2, 3, 4, 5}
    >>> s3 = {2, 8, 9, 1, 7}
    >>> difference(s1, s2, s3)
    {0, 6}
    
    @param args: Arbitrary number of sets.
    @return: Difference between the first set and the rest.

```
### Symmetric_Difference
```pydocstring
Help on function symmetric_difference in module IteratorAlgorithms:

symmetric_difference(*args: set) -> set
    Multiple Set Symmetric Difference
    Includes all elements that are not common to every set passed in.
    If there is no intersection, it will return the union of all sets.
    If all sets are the same, it will return the empty set.
    Opposite of intersection.
    
    DocTests:
    >>> s1 = {0, 2, 4, 6, 8}
    >>> s2 = {1, 2, 3, 4, 5}
    >>> s3 = {2, 8, 9, 1, 7}
    >>> symmetric_difference(s1, s2, s3)
    {0, 1, 3, 4, 5, 6, 7, 8, 9}
    
    @param args: Arbitrary number of sets.
    @return: Symmetric difference considering all sets.

```

## Test Summary
```pydocstring
35 items passed all tests:
   4 tests in __main__
   2 tests in __main__.accumulate
   4 tests in __main__.add_all
   2 tests in __main__.add_one
   2 tests in __main__.adjacent_difference
   4 tests in __main__.all_of
   9 tests in __main__.analytic_continuation
   4 tests in __main__.any_of
   4 tests in __main__.difference
   2 tests in __main__.exclusive_scan
   3 tests in __main__.flatten
   5 tests in __main__.fork
   3 tests in __main__.generate
   2 tests in __main__.generate_n
   2 tests in __main__.inclusive_scan
   2 tests in __main__.inner_product
   4 tests in __main__.intersection
   4 tests in __main__.iota
   5 tests in __main__.is_even
   5 tests in __main__.is_odd
   1 tests in __main__.min_max
   4 tests in __main__.none_of
   2 tests in __main__.partial_sum
   2 tests in __main__.partition
   2 tests in __main__.product
   2 tests in __main__.random_below
   4 tests in __main__.range_test
   2 tests in __main__.reduce
   3 tests in __main__.square
   4 tests in __main__.symmetric_difference
   2 tests in __main__.transform
   2 tests in __main__.transform_reduce
   4 tests in __main__.transposed_sums
   4 tests in __main__.union
   4 tests in __main__.zip_transform
114 tests in 35 items.
114 passed and 0 failed.
Test passed.
```
