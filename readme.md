itpy.py
=========

Built on the back of itertools, `itpy` allows easy list processing through chaining methods.
Everything is a lazy evaluated generating function so nothing happens until you call a method
with side effects. This allows for a fast memory effecient 'keep what you need' way of processing
large ammounts of data!


## Installation

Its not ready for PyPi yet so you'd have to just clone and run the following command.

    python setup.py install

## Getting Started

For syntactic preference, you can either collect the generator into a list by calling `itpy.collect()` or by calling
`itpy._` the `_` essential represents the termination of the transformation.

    ```python
    from itpy import itpy as _

    # apply a map!
    lst = _([1,2,3]).map(lambda x: x**2)._
    # [1,4,9]
    
    # apply a groupby on tuples!
    lst = _([(0, 0), (0, 2), (0, 4), (1, 1), (1, 3), (1, 5)]).groupby()._
    # [(0, [0, 2, 4]),
       (1, [1, 3, 5])]
       
    # apply an arbitrary key function with a groupby!
    lst = _(xrange(0,6).groupby(lambda x: x%2)._
    # [(0, [0, 2, 4]),
       (1, [1, 3, 5])]
       
    # it uses pybloom for memory effecient distincts
    lst = _(huge_list_of_values).distinct()._
    lst = _(huge_list_of_values).distinct_approx()._
    
    ```
    
## Documentation, pydoc is best.

    class Itpy()
     |
     |  cache(self)
     |      Clone the iterable to produce a deepcopy. This may be desired if we wish
     |      to maintain the state of the iterator while also calling a method with side effects.
     |      
     |      :rtype : Itpy
     |      :return:
     |  
     |  collect(self)
     |      Collect the iterable back into a list.
     |      
     |      :rtype : list
     |      :return:
     |  
     |  count(self)
     |      Counts all the elements of the original iterable
     |      
     |      :rtype: int
     |      :return:
     |  
     |  distinct(self)
     |      Make an iterator with only the distinct elements of the previous.
     |      
     |      :rtype : Itpy
     |      :return:
     |  
     |  distinct_approx(self, init_cap=200, err_rate=0.001)
     |      Make an iterator with only the distinct elements of the previous.
     |      Uses a Bloom filter for better space efficiency at the cost of false positive
     |      
     |      :rtype : Itpy
     |      :return:
     |  
     |  dropwhile(self, predicate)
     |      Make an iterator that drops elements from the iterable as long as the predicate is true
     |      
     |      Note, the iterator does not produce any output until the predicate first becomes false,
     |      so it may have a lengthy start-up time.
     |      
     |      :rtype : Itpy
     |      :param predicate:
     |      :return:
     |  
     |  filter(self, predicate)
     |      Make an iterator that filters elements from iterable returning only those for which the predicate is True.
     |      If predicate is None, return the items that are true.
     |      
     |      :rtype : Itpy
     |      :param predicate:
     |      :return:
     |  
     |  filterfalse(self, predicate)
     |      Make an iterator that filters elements from iterable returning only those for which the predicate is False.
     |      If predicate is None, return the items that are false
     |      
     |      :rtype : Itpy
     |      :param predicate:
     |      :return:
     |  
     |  flatmap(self, function_to_list)
     |      Make an interator that returns elements from the lists produced by mapping function_to_list.
     |      
     |      :rtype : Itpy
     |      :param function_to_list:
     |      :return:
     |  
     |  groupby(self, key=None, value=None)
     |      Make an iterator that returns consecutive keys and groups from the iterable.
     |      The key and value is computed each element by keyfunc and valfunc.
     |      If these functions are not not specified or is None, they default to identity function.
     |      
     |      :rtype : Itpy
     |      :param key:
     |      :param value:
     |      :return:
     |  
     |  map(self, function)
     |      Make an iterator that computes the function using arguments from each of the iterables.
     |      
     |      :rtype : Itpy
     |      :param func:
     |      :return:
     |  
     |  reduce(self, reducing_function)
     |      Get a merged value using an associative reduce function,
     |      so as to reduce the iterable to a single value from left to right.
     |      
     |      :param reducing_function:
     |      :return:
     |  
     |  reduce_pair_by_key(self, reducing_function)
     |      Make an iterator that returns the merged values for each key using an associative reduce function.
     |      The values contained in this Iter must be 2-tuples in the form (k, v) where v is Iterable.
     |      
     |      :rtype: Itpy
     |      :param reducing_function:
     |      :return:
     |  
     |  reduce_values(self, reducing_function)
     |      Make an iterator that returns the merged values for each key using an associative reduce function.
     |      The values contained in this Iter must be 2-tuples in the form (k, v) where v is Iterable.
     |      
     |      :rtype: Itpy
     |      :param reducing_function:
     |      :return:
     |  
     |  slice(self, *args)
     |      Make an iterator that returns selected elements from the iterable. If start is non-zero, then elements from
     |      the iterable are skipped until start is reached. Afterward, elements are returned consecutively unless step is
     |      set higher than one which results in items being skipped. If stop is None, then iteration continues until the
     |      iterator is exhausted, if at all; otherwise, it stops at the specified position. Unlike regular slicing,
     |      slice() does not support negative values for start, stop, or step.
     |      
     |      :rtype : Itpy
     |      :param args:
     |      :return:
     |  
     |  sort(self, cmp=None, key=None, reverse=False)
     |      Make and iterator that is sorted on a specific key, if key is None, sort on
     |      natural ordering.
     |      
     |      :rtype: Itpy
     |      :param key:
     |      :return:
     |  
     |  take(self, max)
     |      Make and iterator that returns the first max elements from the original iterable
     |      
     |      :rtype : Itpy
     |      :param max:
     |      :return:
     |  
     |  takewhile(self, predicate)
     |      Make an iterator that returns elements from the iterable as long as the predicate is true.
     |      
     |      :rtype : Itpy
     |      :param predicate:
     |      :return:
     |  
     |  top(self, k=1, key=None)
     |      Make an iterable of the top k elements of the original iterable sorted on keyfunc, if keyfunc is None sort
     |      on the natural ordering.
     |      
     |      :rtype: Itpy
     |      :param k:
     |      :return:
     |  
     |  union(self, *iterable)
     |      Make an iterator that returns elements from the first iterable until it
     |      is exhausted, then proceeds to the next iterable, until all of the iterables
     |      are exhausted. Used for treating consecutive sequences as a single sequence.
     |      
     |      :rtype Iter:
     |      :param iterable:
     |      :return:

## Digging deeper

This is partly inspired by how much fun it was to write spark jobs. If you want to request a feature or submit a feature
, write up a method, write up some tests and make a pull request! If you implemented an interesting algorithm in spark
try writing it in this and show it off!