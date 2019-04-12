---
title: "Running scripts and importing your own functions"
teaching: 20
exercises: 0
questions:
- "How do I run scripts from the command line?"
- "Hopw do I import my own functions as modules?"
objectives:
- "Converting programs to script files and run them."
- "Reuse your own code by importing it as a module."
keypoints:
- "There are multiple ways to run programs."
---


# Work In Progress

## Create a script file in Jupyter and run it

* open terminal from Notebook
* run `jupyter-nbconvert --to python my_book.ipynb`
* run `python my_book.py`
* can use Python text editor from Jupyter

## Getting input from the command line

Create a script that is called "sys-test.py". Put the following content into it:

```Python
import sys
print(sys.argv)
for element in sys.argv:
    print(element)
print(sys.argv[1])
```
Run the commands shown below on the command line:

  * python sys-test.py 1
  * python sys-test first second
  * python sys-test.py 1 2 3 4
  * python sys-test.py

Question: can you figure out why the last one fails?


## Import functions from script files

## Dual usage of script files

`if __name__ == "__main__":`
