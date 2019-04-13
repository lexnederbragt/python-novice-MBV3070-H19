---
title: "Using BioPython"
teaching: 0
exercises: 15
questions:
- "What and how can we use the BioPython library?"
objectives:
- "Basic understanding of BioPython usage"
keypoints:
- "BioPython contains many useful functions for bioinformatics."
---

## What is Biopython

From [biopython.org](https://biopython.org): "Biopython is a set of freely available tools for biological computation written in Python by an international team of developers."


## An example function
~~~
from Bio.SeqUtils import GC
GC("ACTGN")
~~~
{: .python}

The source code of this function can be found on [Github](https://github.com/biopython/biopython/), in the folder `Bio`, folder `SeqUtils`, file `__init__.py` ([direct link](https://github.com/biopython/biopython/blob/master/Bio/SeqUtils/__init__.py)).

~~~
def GC(seq):
    """Calculate G+C content, return percentage (as float between 0 and 100).
    Copes mixed case sequences, and with the ambiguous nucleotide S (G or C)
    when counting the G and C content.  The percentage is calculated against
    the full length, e.g.:
    >>> from Bio.SeqUtils import GC
    >>> GC("ACTGN")
    40.0
    Note that this will return zero for an empty sequence.
    """
    gc = sum(seq.count(x) for x in ['G', 'C', 'g', 'c', 'S', 's'])
    try:
        return gc * 100.0 / len(seq)
    except ZeroDivisionError:
        return 0.0
~~~
{: .python}


See also the file [test_SeqUtils.py](https://github.com/biopython/biopython/blob/master/Tests/test_SeqUtils.py#L139) where the GC function is *tested*.

It is good practice to add some tests, positive and negative controls, for a function. A simpler way to do this is to add `asserts`:

~~~
def test_gc():
    seq = "ACGGGCTACCGTATAGGCAAGAGATGATGCCC"
    assert GC(seq) == 56.25
~~~
{: .python}

Run as:

~~~
test_gc()
~~~
{: .python}

There are programs that will automatically run all tests in a module (package) and report on which ones failed.

## Working with sequences

Adapted from the [Biopython wiki](https://biopython.org/wiki/Converting_sequence_files).

Suppose you have a `GenBank` file which you want to turn into a Fasta file. For example, let’s consider the file `cor6_6.gb` which corresponds to GenBank file [X55053](https://www.ncbi.nlm.nih.gov/nuccore/X55053). This file is included in the `data` folder. Study the file: there is a lot of information there. How many gene sequences are in this file?

If you want to extract the sequences into a file in `fasta` format, you'd have to write quite a bit of Python code. Luckily, this has already been done by the Biopython developers. We can use this code as follows:

~~~
from Bio import SeqIO

sequences = SeqIO.parse("cor6_6.gb", "genbank")
count = SeqIO.write(sequences, "cor6_6.fasta", "fasta")

print("Converted %i records" % count)
~~~
{: .python}

An excellent tutorial on using BioPython can be found [on Github](https://github.com/peterjc/biopython_workshop).
