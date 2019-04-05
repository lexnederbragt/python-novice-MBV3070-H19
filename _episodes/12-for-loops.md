---
title: "For Loops"
teaching: 10
exercises: 15
questions:
- "How can I make a program do many things?"
objectives:
- "Explain what for loops are normally used for."
- "Trace the execution of a simple (unnested) loop and correctly state the values of variables in each iteration."
- "Write for loops that use the Accumulator pattern to aggregate values."
keypoints:
- "A *for loop* executes commands once for each value in a collection."
- "A `for` loop is made up of a collection, a loop variable, and a body."
- "The first line of the `for` loop must end with a colon, and the body must be indented."
- "Indentation is always meaningful in Python."
- "Loop variables can be called anything (but it is strongly advised to have a meaningful name to the looping variable)."
- "The body of a loop can contain many statements."
- "Use `range` to iterate over a sequence of numbers."
- "The Accumulator pattern turns many values into one."
---
## A *for loop* executes commands once for each value in a collection.

*   Doing calculations on the values in a list one by one
    is as painful as working with `pressure_001`, `pressure_002`, etc.
*   A *for loop* tells Python to execute some statements once for each value in a list,
    a character string,
    or some other collection.
*   "for each thing in this group, do these operations"

~~~
for number in [2, 3, 5]:
    print(number)
~~~
{: .python}

*   This `for` loop is equivalent to:

~~~
print(2)
print(3)
print(5)
~~~
{: .python}

*   And the `for` loop's output is:

~~~
2
3
5
~~~
{: .output}

## A `for` loop is made up of a collection, a loop variable, and a body.

~~~
for number in [2, 3, 5]:
    print(number)
~~~
{: .python}

*   The collection, `[2, 3, 5]`, is what the loop is being run on.
*   The body, `print(number)`, specifies what to do for each value in the collection.
*   The loop variable, `number`, is what changes for each *iteration* of the loop.
    *   The "current thing".

## The first line of the `for` loop must end with a colon, and the body must be indented.

*   The colon at the end of the first line signals the start of a *block* of statements.
*   Python uses indentation rather than `{}` or `begin`/`end` to show *nesting*.
    *   Any consistent indentation is legal, but almost everyone uses four spaces.

~~~
for number in [2, 3, 5]:
print(number)
~~~
{: .python}
~~~
IndentationError: expected an indented block
~~~
{: .error}

*   Indentation is always meaningful in Python.

~~~
firstName="Jon"
  lastName="Smith"
~~~
{: .python}
~~~
  File "<ipython-input-7-f65f2962bf9c>", line 2
    lastName="Smith"
    ^
IndentationError: unexpected indent
~~~
{: .error}

*   This error can be fixed by removing the extra spaces
    at the beginning of the second line.

## Loop variables can be called anything.

*   As with all variables, loop variables are:
    *   Created on demand.
    *   Meaningless: their names can be anything at all.

~~~
for kitten in [2, 3, 5]:
    print(kitten)
~~~
{: .python}

## The body of a loop can contain many statements.

*   But no loop should be more than a few lines long.
*   Hard for human beings to keep larger chunks of code in mind.

~~~
primes = [2, 3, 5]
for p in primes:
    squared = p ** 2
    cubed = p ** 3
    print(p, squared, cubed)
~~~
{: .python}
~~~
2 4 8
3 9 27
5 25 125
~~~
{: .output}

Let's visualize this loop using Python Tutor. In the notebook, write

~~~
%load tutor.py
~~~
{: .python}

This loads the following code:

~~~
from metakernel import register_ipython_magics
register_ipython_magics()
~~~
{: .python}

Execute the code (it gives no output).

Now you can rerun the previous for loop in a new cell that starts with `%%tutor`:
~~~
%%tutor
primes = [2, 3, 5]
for p in primes:
    squared = p ** 2
    cubed = p ** 3
    print(p, squared, cubed)
~~~
{: .python}

## Use `range` to iterate over a sequence of numbers.

*   The built-in function `range` produces a sequence of numbers.
    *   *Not* a list: the numbers are produced on demand
        to make looping over large ranges more efficient.
*   `range(N)` is the numbers 0..N-1
    *   Exactly the legal indices of a list or character string of length N

~~~
print('a range is not a list: range(0, 3)')
for number in range(0,3):
    print(number)
~~~
{: .python}
~~~
a range is not a list: range(0, 3)
0
1
2
~~~
{: .output}

## A string is also a collection that can be looped over.

~~~
organ = "liver"
for char in organ:
    print(char)
~~~
{: .python}

This means that if we have a DNA string, we can loop over the individual bases:

~~~
DNA = "ACGTGC"
for base in DNA:
    print(base)
~~~
{: .python}

As an example, let's use a for loop to de the same as the `.upper()` function. `.upper()` changes all characters to upper case:


~~~
dna1 = "acgtgc"
print(dna1)
dna2 = dna1.upper()
print(dna2)
~~~
{: .python}

With a for loop, we could do it like this:

~~~
dna1 = "acgtgc"
dna2 = ''
print(dna1)
for base in dna1:
    dna2 = dna2 + base.upper()
print(dna2)
~~~
{: .python}

Rerun this loop using Python Tutor as described above.

## The Accumulator pattern turns many values into one.

*   A common pattern in programs is to:
    1.  Initialize an *accumulator* variable to zero, the empty string, or the empty list.
    2.  Update the variable with values from a collection.

~~~
# Sum the first 10 integers.
total = 0
for number in range(10):
   total = total + (number + 1)
print(total)
~~~
{: .python}
~~~
55
~~~
{: .output}

*   Read `total = total + (number + 1)` as:
    *   Add 1 to the current value of the loop variable `number`.
    *   Add that to the current value of the accumulator variable `total`.
    *   Assign that to `total`, replacing the current value.
*   We have to add `number + 1` because `range` produces 0..9, not 1..10.

## Exercises on slides
* Reversing a String
* Summarising DNA strings
* Cumulative Sum

<!--

> ## Classifying Errors
>
> Is an indentation error a syntax error or a runtime error?
> > ## Solution
> > An IndentationError is a syntax error. Programs with syntax errors cannot be started.
> > A program with a runtime error will start but an error will be thrown under certain conditions.
> {: .solution}
{: .challenge}

> ## Tracing Execution
>
> Create a table showing the numbers of the lines that are executed when this program runs,
> and the values of the variables after each line is executed.
>
> ~~~
> total = 0
> for char in "tin":
>     total = total + 1
> ~~~
> {: .python}
> > ## Solution
> >
> > | Line no | Variables            |
> > |---------|----------------------|
> > | 1       | total = 0            |
> > | 2       | total = 0 char = 't' |
> > | 3       | total = 1 char = 't' |
> > | 2       | total = 1 char = 'i' |
> > | 3       | total = 2 char = 'i' |
> > | 2       | total = 2 char = 'n' |
> > | 3       | total = 3 char = 'n' |
> {: .solution}
{: .challenge}

> ## Identifying Variable Name Errors
>
> 1. Read the code below and try to identify what the errors are
>    *without* running it.
> 2. Run the code and read the error message.
>    What type of `NameError` do you think this is?
>    Is it a string with no quotes, a misspelled variable, or a
>    variable that should have been defined but was not?
> 3. Fix the error.
> 4. Repeat steps 2 and 3, until you have fixed all the errors.
>
> ~~~
> for number in range(10):
>     # use a if the number is a multiple of 3, otherwise use b
>     if (Number % 3) == 0:
>         message = message + a
>     else:
>         message = message + "b"
> print(message)
> ~~~
> {: .python}
> > ## Solution
> > ~~~
> > message = ""
> > for number in range(10):
> >     # use a if the number is a multiple of 3, otherwise use b
> >     if (number % 3) == 0:
> >         message = message + "a"
> >     else:
> >         message = message + "b"
> > print(message)
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}

> ## Identifying Item Errors
>
> 1. Read the code below and try to identify what the errors are
>    *without* running it.
> 2. Run the code, and read the error message. What type of error is it?
> 3. Fix the error.
>
> ~~~
> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
> print('My favorite season is ', seasons[4])
> ~~~
> {: .python}
> > ## Solution
> > ~~~
> > seasons = ['Spring', 'Summer', 'Fall', 'Winter']
> > print('My favorite season is ', seasons[3])
> > ~~~
> > {: .python}
> {: .solution}
{: .challenge}
-->
