---
title: "Lists"
teaching: 10
exercises: 10
questions:
- "How can I store multiple values?"
objectives:
- "Explain why programs need collections of values."
- "Write programs that create flat lists, index them, slice them, and modify them through assignment and method calls."
keypoints:
- "A list stores many values in a single structure."
- "Use an item's index to fetch it from a list."
- "Lists' values can be replaced by assigning to them."
- "Appending items to a list lengthens it."
- "Use `del` to remove items from a list entirely."
- "The empty list contains no values."
- "Lists may contain values of different types."
- "Character strings can be indexed like lists."
- "Character strings are immutable."
- "Indexing beyond the end of the collection is an error."
---
## A list stores many values in a single structure.

*   Doing calculations with a hundred variables called `pressure_001`, `pressure_002`, etc.,
    would be at least as slow as doing them by hand.
*   Use a *list* to store many values together.
    *   Contained within square brackets `[...]`.
    *   Values separated by commas `,`.
*   Use `len` to find out how many values are in a list.

~~~
pressures = [0.273, 0.275, 0.277, 0.275, 0.276]
print('pressures:', pressures)
print('length:', len(pressures))
~~~
{: .python}
~~~
pressures: [0.273, 0.275, 0.277, 0.275, 0.276]
length: 5
~~~
{: .output}

## Use an item's index to fetch it from a list.

*   Just like strings.

~~~
print('zeroth item of pressures:', pressures[0])
print('fourth item of pressures:', pressures[4])
~~~
{: .python}
~~~
zeroth item of pressures: 0.273
fourth item of pressures: 0.276
~~~
{: .output}

## Lists' values can be replaced by assigning to them.

*   Use an index expression on the left of assignment to replace a value.

~~~
pressures[0] = 0.265
print('pressures is now:', pressures)
~~~
{: .python}
~~~
pressures is now: [0.265, 0.275, 0.277, 0.275, 0.276]
~~~
{: .output}

## Appending items to a list lengthens it.

*   Use `list_name.append` to add items to the end of a list.

~~~
primes = [2, 3, 5]
print('primes is initially:', primes)
primes.append(7)
primes.append(9)
print('primes has become:', primes)
~~~
{: .python}
~~~
primes is initially: [2, 3, 5]
primes has become: [2, 3, 5, 7, 9]
~~~
{: .output}

*   `append` is a *method* of lists.
    *   Like a function, but tied to a particular object.
*   Use `object_name.method_name` to call methods.
    *   Deliberately resembles the way we refer to things in a library.
*   We will meet other methods of lists as we go along.
    *   Use `help(list)` for a preview.
*   `extend` is similar to `append`, but it allows you to combine two lists.  For example:

~~~
teen_primes = [11, 13, 17, 19]
middle_aged_primes = [37, 41, 43, 47]
print('primes is currently:', primes)
primes.extend(teen_primes)
print('primes has now become:', primes)
primes.append(middle_aged_primes)
print('primes has finally become:', primes)
~~~
{: .python}
~~~
primes is currently: [2, 3, 5, 7, 9]
primes has now become: [2, 3, 5, 7, 9, 11, 13, 17, 19]
primes has finally become: [2, 3, 5, 7, 9, 11, 13, 17, 19, [37, 41, 43, 47]]
~~~
{: .output}

Note that while `extend` maintains the "flat" structure of the list, appending a list to a list makes the result two-dimensional.

## Use `del` to remove items from a list entirely.

*   `del list_name[index]` removes an item from a list and shortens the list.
*   Not a function or a method, but a statement in the language.

~~~
print('primes before removing last item:', primes)
del primes[4]
print('primes after removing last item:', primes)
~~~
{: .python}
~~~
primes before removing last item: [2, 3, 5, 7, 9]
primes after removing last item: [2, 3, 5, 7]
~~~
{: .output}

## The empty list contains no values.

*   Use `[]` on its own to represent a list that doesn't contain any values.
    *   "The zero of lists."
*   Helpful as a starting point for collecting values
    (which we will see in the [next episode]({{page.root}}/09-for-loops/)).

## Lists may contain values of different types.

*   A single list may contain numbers, strings, and anything else.

~~~
goals = [1, 'Create lists.', 2, 'Extract items from lists.', 3, 'Modify lists.']
~~~
{: .python}

## Character strings can be indexed like lists.

*   Get single characters from a character string using indexes in square brackets.

~~~
element = 'carbon'
print('zeroth character:', element[0])
print('third character:', element[3])
~~~
{: .python}
~~~
zeroth character: c
third character: b
~~~
{: .output}

## Character strings are immutable.

*   Cannot change the characters in a string after it has been created.
    *   *Immutable*: can't be changed after creation.
    *   In contrast, lists are *mutable*: they can be modified in place.
*   Python considers the string to be a single value with parts,
    not a collection of values.

~~~
element[0] = 'C'
~~~
{: .python}
~~~
TypeError: 'str' object does not support item assignment
~~~
{: .error}

*   Lists and character strings are both *collections*.

## Indexing beyond the end of the collection is an error.

*   Python reports an `IndexError` if we attempt to access a value that doesn't exist.
    *   This is a kind of [runtime error]({{ page.root }}/05-error-messages/).
    *   Cannot be detected as the code is parsed
        because the index might be calculated based on data.

~~~
print('99th element of element is:', element[99])
~~~
{: .python}
~~~
IndexError: string index out of range
~~~
{: .output}

> ## Working With the End
>
> What does the following program print?
>
> ~~~
> element = 'helium'
> print(element[-1])
> ~~~
> {: .python}
>
> 1.  How does Python interpret a negative index?
> 2.  If a list or string has N elements,
>     what is the most negative index that can safely be used with it,
>     and what location does that index represent?
> 3.  If `values` is a list, what does `del values[-1]` do?
> 4.  How can you display all elements but the last one without changing `values`?
>     (Hint: you will need to combine slicing and negative indexing.)
>
> > ## Solution
> > The program prints `m`.
> > 1. Python interprets a negative index as starting from the end (as opposed to
> >    starting from the beginning).  The last element is `-1`.
> > 2. The last index that can safely be used with a list of N elements is element
> >    `-N`, which represents the first element.
> > 3. `del values[-1]` removes the last element from the list.
> > 4. `values[:-1]`
> {: .solution}
{: .challenge}

## Dictionaries contain key-value pairs

<!-- from https://www.thegeekstuff.com/2013/05/python-dictionary/?utm_source=tuicool -->

The last kind of new data structure we'll introduce is the Dictionary.

Here is an example of how we can create a dictionary in Python:

~~~
myDict = {"A":"Apple", "B":"Boy", "C":"Cat"}
~~~
{: .python}

In the above example:

- A dictionary is created.
- This dictionary contains three elements.
- Each element constitutes of a key value pair.
- This dictionary can be accessed using the variable myDict.

If you just type the name of the variable myDict, all the key value pairs in the dictionary will be printed.

~~~
myDict
~~~
{: .python}
~~~
{'A': 'Apple', 'C': 'Cat', 'B': 'Boy'}
~~~
{: .output}


### Access Dictionary Elements

Once a dictionary is created, you can access it using the variable to which it is assigned during creation. For example, in our case, the variable myDict can be used to access the dictionary elements.

Here is how this can be done:

~~~
myDict["A"]
myDict["B"]
myDict["C"]
~~~
{: .python}
~~~
'Apple'
'Boy'
'Cat'
~~~
{: .output}

So you can see that using the variable myDict and Key as index, the value of corresponding key can be accessed.


There is one thing that you need to keep in your mind. Only dictionary keys can be used as indexes. This means that myDict[“A”] would produce ‘Apple’ in output but myDict[“Apple”] cannot produce ‘A’ in the output.

~~~
myDict["Apple"]
~~~
{: .python}
~~~
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Apple'
~~~
{: .output}

So we see that Python complained about ‘Apple’ being used as index.

### Update Dictionary Elements

Just the way dictionary values are accessed using keys, the values can also be modified using the dictionary keys. Here is an example to modify python dictionary element:

~~~
myDict["A"] = "Application"
myDict["A"]
myDict
~~~
{: .python}
~~~
'Application'
{'A': 'Application', 'C': 'Cat', 'B': 'Boy'}
~~~
{: .output}

You can see that in the example shown above, the value of key ‘A’ was changed from ‘Apple’ to ‘Application’ easily.  This way we can easily conclude that there could not be two keys with same name in a dictionary.

### Delete Dictionary Elements

Individual elements can be deleted easily from a dictionary. Here is an example to remove an element from dictionary.

~~~
del myDict["A"]
myDict
~~~
{: .python}
~~~
{'C': 'Cat', 'B': 'Boy'}
~~~
{: .output}

So you can see that by using ‘del’ an element can easily be deleted from the dictionary.

If you want to delete complete dictionary ie all the elements in the dictionary then it can be done using the clear() function. Here is an example :

~~~
myDict.clear()
myDict
~~~
{: .python}
~~~
{}
~~~
{: .output}

So you see that all the elements were deleted making the dictionary empty.

This naturally leads to the command to set up an empty dictionary:

~~~
newDict = {}
newDict
~~~
{: .python}
~~~
{}
~~~
{: .output}


## Exercises on slides
* Uses for dictionaries in bioinformatics
* Gjør ferdig koden: lister


<!--
> ## How Large is a Slice?
>
> If 'low' and 'high' are both non-negative integers,
> how long is the list `values[low:high]`?
>
> > ## Solution
> > The list `values[low:high]` has `high - low` elements.  For example,
> > `values[1:4]` has the 3 elements `values[1]`, `values[2]`, and `values[3]`.
> > Note that the expression will only work if `high` is less than the total
> > length of the list `values`.
> {: .solution}
{: .challenge}

> ## From Strings to Lists and Back
>
> Given this:
>
> ~~~
> print('string to list:', list('tin'))
> print('list to string:', ''.join(['g', 'o', 'l', 'd']))
> ~~~
> {: .python}
> ~~~
> ['t', 'i', 'n']
> 'gold'
> ~~~
> {: .output}
>
> 1.  Explain in simple terms what `list('some string')` does.
> 2.  What does `'-'.join(['x', 'y'])` generate?
>
> > ## Solution
> > 1. `list('some string')` "splits" a string into a list of its characters.
> > 2. `x-y`
> {: .solution}
{: .challenge}

-->
