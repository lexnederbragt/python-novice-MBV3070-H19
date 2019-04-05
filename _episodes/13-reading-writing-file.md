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

### Reading FASTA files with the open-function

The hemoglobin sequences are so large, that we should not store them directly in the code.
For this reason we have stored them in two files, one named `hemoglobin_normal.fasta` and
one named `hemoglobin_sickle.fasta`.

Python has a function for opening any file, named `open()`:
<!--  -->

~~~
f = open("hemoglobin_normal.fasta", "r")
~~~
{: .python}

<!--  -->
The first argument to `open()` is the name of the file we want to open given as a
string.
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

<!--  -->

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

<!--  -->
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
<!--  -->

~~~
f.close()
~~~
{: .python}

<!--  -->

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

<!--  -->

Strings have a method called `.strip()`, which removes all whitespace (tabs,
newlines, and spaces) at the beginning and the end of the string:
<!--  -->

~~~
my_string = "      spaces and newlines   \n\n\n"

print("With spaces and newlines:")
print(my_string)
print("Without spaces and newlines:")
print(my_string.strip())
~~~
{: .python}

<!--  -->

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

<!--  -->

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
Writing to a file is quite similar to writing text to the terminal.
We can write to a file by opening it with the argument `"w"` (for write) or
`"a"` (for append):



~~~
f = open("our_file.txt", "w")
~~~
{: .python}

<!--  -->

If the file already exists, and we want to keep the existing content,
we must use the `"a"` argument.
Opening the file with the `"w"` argument removes all content from the file.

We write to the file with `f.write()`, which returns the number of characters
written to the file:


~~~
f.write("The first line.\n")
~~~
{: .python}

<!--  -->

~~~
f.write("The second line.")
~~~
{: .python}

<!--  -->
`f.write()` does not automatically give us newlines from one function call to the next.
To get a newline we therefore have to write `\n` at the end of our string.

We have to close the file once we are finished writing to it:
<!--  -->

~~~
f.close()
~~~
{: .python}

<!--  -->

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

<!--  -->
This tells us we have `ValueError` because we try an
`I/O operation on closed file.`
This means we are trying to do an input or output operation,
such as reading from or writing to a file that is closed.



<!-- --- begin exercise --- -->

### Exercise 1: write something about yourself to a file


*a)*
Skiv noe tekst, for eksempel om deg selv, til en ny fil, og les filen

<!-- --- end exercise --- -->
