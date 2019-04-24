---
title: "Reading from and writing to files"
teaching: 5
exercises: 10
questions:
- "How can I read data from, and write to files?"
objectives:
- "Be able to read and write from and to files."
keypoints:
- ""
---

> ## About this version
>
> This episode replaces to original episode [Looping Over Data Sets](http://swcarpentry.github.io/python-novice-gapminder/13-looping-data-sets/index.html).
{: .callout}


Hemoglobin is the protein responsible for binding oxygen in red blood cells.
It consists of four smaller proteins, called subunits.
[Sickle-cell anemia](https://en.wikipedia.org/wiki/Sickle_cell_disease) occurs because of a substitution in the DNA sequence that
codes for one of these subunits.
A single base in the hemoglobin DNA is changed and the result is a genetic
disorder where the red blood cells assume a sickle-like shape.

In this episode, we will learn how to read sequences for normal and sickle-cell anemia causing hemoglobins from files. After that, we will compare them and find the substitution between them.

### Reading FASTA files with the open-function

The hemoglobin sequences are so large, that we should not store them directly in the code.
For this reason we have stored them in two files, one named `hemoglobin_normal.fasta` and
one named `hemoglobin_sickle.fasta`. These files are stored in the folder called `data`.

Python has a function for opening any file, named `open()`:

~~~
f = open("data/hemoglobin_normal.fasta", "r")
~~~
{: .python}

The first argument to `open()` is the name of the file we want to open given as a
string. We need to add the `data` folder in front of it, followed by a forward slash `/`.
The second argument, the string `"r"` (for read),
tells that we want to open the file for reading.
The content of the file `f` can be read in various ways,
but a common method is to use the function `readlines()`.
This function returns a list where each line in our file is an element in the lists.


~~~
lines = f.readlines()
print(lines)
~~~
{: .python}


This data is stored in the FASTA format.
FASTA is a popular file format used for storing DNA sequences.
It is a text based format where the first line starts with a `>`
(greater-than symbol) and contains different meta data for that specific sequence.
This line is called a header.
Our mRNA hemoglobin sequence files have the following header:


~~~
>NM_000518.4 Homo sapiens hemoglobin subunit beta (HBB), mRNA
~~~
{: .output}

This header tells us we have mRNA for a specific subunit of hemoglobin for
humans.
We do not need the header for our purpose, because we already know we are
working with hemoglobin from humans.
We are interested in the mRNA sequence itself.

Each line in the output above ends with the characters `\n`.
This is the *newline* symbol and signifies a new line.
When we print a string, the symbol `\n` is converted to a new line:


~~~
print("Hello\nWorld!")
~~~
{: .python}

We should always close a file when we are finished working with it.
We do this by using the `close()` method:

~~~
f.close()
~~~
{: .python}


We would like to work with one string for the entire mRNA sequence.
We turn `lines` into one string by starting with an empty string called `mRNA`,
looping over each element of `lines`, and adding them to the `mRNA` string.
Each element in `lines` is a string and joining strings is done with `+` (the
addition symbol).
After the loop we print the result:


~~~
mRNA = ""
for line in lines:
    mRNA += line

print(mRNA)
~~~
{: .python}


Strings have a method called `.strip()`, which removes all whitespace (tabs,
newlines, and spaces) at the beginning and the end of the string:

~~~
my_string = "      spaces and newlines   \n\n\n"

print("With spaces and newlines:")
print(my_string)
print("Without spaces and newlines:")
print(my_string.strip())
~~~
{: .python}


We use `strip()` to remove the newline symbols from each line in `lines`.
We also want to ignore the first line with the header,
and loop over all lines from the second line to the last with `lines[1:]`:


~~~
mRNA = ""
for line in lines[1:]:
    mRNA += line.strip()

print(mRNA)
~~~
{: .python}


~~~
ACAUUUGCUUCUGACACAAC ...(Truncated)... AAAACAUUUAUUUUCAUUGC
~~~
{: .output}

<!-- #else -->


The output of this code is a single string (without whitespaces) that contains
the entire mRNA sequence.

The full code for reading a sequence from FASTA files is then:


~~~
f = open(filename)
lines = f.readlines()
f.close()

mRNA = ""
for line in lines[1:]:
    mRNA += line.strip()

~~~
{: .python}

### Writing to files

We have until now only read data from a file,
but it is also useful to know how to write information to a file.
Writing to a file is quite similar to writing text to the notebook.
We can write to a file by opening it with the argument `"w"` (for write) or
`"a"` (for append):



~~~
f = open("our_file.txt", "w")
~~~
{: .python}


If the file already exists, and we want to keep the existing content,
we must use the `"a"` argument.
Opening the file with the `"w"` argument removes all content from the file.

We write to the file with `f.write()`, which returns the number of characters
written to the file:


~~~
f.write("The first line.\n")
~~~
{: .python}


~~~
f.write("The second line.")
~~~
{: .python}

`f.write()` does not automatically give us newlines from one function call to the next.
To get a newline we therefore have to write `\n` at the end of our string.

We have to close the file once we are finished writing to it:

~~~
f.close()
~~~
{: .python}


If we take a look at the contents of `our_file.txt`, we find the following:


~~~
The first line.
The second line.
~~~
{: .python}

If you try to read from a closed file, you get the following error:


~~~
f = open("our_file.txt", "w")
f.close()

lines = f.readlines()
~~~
{: .python}

This tells us we have `ValueError` because we try an
`I/O operation on closed file.`
This means we are trying to do an input or output operation,
such as reading from or writing to a file that is closed.


### Comparing two mRNA strands

We know have loaded the two mRNA strands, and we want to loop through both mRNA
strings simultaneously, nucleotide by nucleotide.
This can be done using `for` loops (see the box below),
but the simplest solution is to use a `while` loop.
We loop over all indices, and access the corresponding nucleotide in each mRNA string,
and if they differ we have found the mutation site.


~~~
index = 0
while index < len(mRNA_original):
   normal = mRNA_original[index]
   sickle = mRNA_mutation[index]

   if normal != sickle:
       print("Mutation has switched", normal, "to", sickle, "at", index)

   index += 1
~~~
{: .python}

There is a substitution from `A` to `U` in the mRNA for a hemoglobin subunit,
and the corresponding substitution in the DNA is from an `A` to a `T`.
In order to identify a suitable restriction enzyme for the restriction cutting,
we need the sequence surrounding the mutation.
To do this, we print the five characters before and after the index for the
mutation location when we find the mutation.


~~~
index = 0
while index < len(mRNA_original):
   normal = mRNA_original[index]
   sickle = mRNA_mutation[index]

   if normal != sickle:
       print("Mutation has switched", normal, "to", sickle, "at", index)

       print("Surrounding sequence, original:", mRNA_original[index - 5:index + 6])
       print("Surrounding sequence, mutation:", mRNA_mutation[index - 5:index + 6])

   index += 1
~~~
{: .python}

Notice that we put the `index += 1` statement at the end of the code block.
This is to make sure we do not change the current index before we print it.
Further, we slice from `index - 5` to `index + 6` because slicing goes from the start
index to, but not including, the stop index.

---
<!-- Comparing two mRNA strands using a `for` loop and the zip-function -->

We know how to loop through one string at the time with a `for` loop,
but here we want to loop through both mRNA strings simultaneously,
nucleotide by nucleotide.
It is possible loop over multiple lists at the same time using the `zip()`
function.
An example of the use of `zip()` is:



~~~
list_a = [1, 2, 3, 4]
list_b = [10, 20, 30, 40]

for a, b in zip(list_a, list_b):
    print(a, b)
~~~
{: .python}

Note how the `a` and `b` variables are assigned the values of the
two lists in turn.
If the two lists are not of equal length,
the loop stops when the end of the shortest list is reached:

~~~
list_a = [1, 2, 3, 4]
list_b = [10, 20, 30, 40, 50, 60]

for a, b in zip(list_a, list_b):
    print(a, b)
~~~
{: .python}


We loop over our two hemoglobin mRNA strands
using `zip()` in the same way,
and test if each nucleotide in the two sequences are equal.
If they differ, we print the nucleotides:

~~~
for normal, sickle in zip(mRNA_original, mRNA_mutation):
    if normal != sickle:
        print("Mutation has switched", normal, "to", sickle)
~~~
{: .python}

## Exercise: what does the substitution do?

Now you know what base was changed between the two versions of the hemoglobin sequence, can you, using Python, find out what amino acid changes, if any, the difference results in? 
