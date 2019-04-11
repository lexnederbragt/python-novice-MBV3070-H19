---
title: "Plotting"
teaching: 15
exercises: 15
questions:
- "How can I plot my data?"
- "How can I save my plot for publishing?"
- "How do I develop a program from scratch?"
objectives:
- "Create a scatter plot showing relationship between two data sets."
keypoints:
- "[`matplotlib`](https://matplotlib.org/) is the most widely used scientific plotting library in Python."
- "Programs are developed in steps"
- "Modularise program design using functions"
- "Can plot many sets of data together."
---

**NOTE** this is a incompletely developed lesson, for example, the output of the different commands is not present.

## [`matplotlib`](https://matplotlib.org/) is the most widely used scientific plotting library in Python.

*   Commonly use a sub-library called [`matplotlib.pyplot`](https://matplotlib.org/api/pyplot_api.html).
*   The Jupyter Notebook will render plots inline if we ask it to using a "magic" command.

~~~
%matplotlib inline
import matplotlib.pyplot as plt
~~~
{: .language-python}

*   Simple plots are then (fairly) simple to create.

~~~
time = [0, 1, 2, 3]
position = [0, 100, 200, 300]

plt.plot(time, position)
plt.xlabel('Time (hr)')
plt.ylabel('Position (km)')
~~~
{: .language-python}

![Simple Position-Time Plot](../fig/9_simple_position_time_plot.svg)


## Developing a program to plot GC Percentage of a bacterial genome

We want to create a plot of the frequency distribution of GC% of a genome in 100 bp windows.
Goals:
* read in the genome sequence
* split up into 100 nucleotide windows
* calculate %GC for each window
* plot the frequency of GC%s


#### Reading in the genome

We already know how to a fasta file. This time, we use genomic DNA, not RNA.


~~~
filename = 'data/NC_000913_E_coli_K12.fasta'
f = open(filename)
lines = f.readlines()
f.close()

print("Identifier:")
print(lines[0])
genome = ""
for line in lines[1:]:
    genome += line.strip()
print(len(genome))
~~~
{: .language-python}

#### Creating non-overlapping windows

We want to develop a program that splits a DNA sequence into 100 bp, non overlapping windows. It is always a good idea to start with a simpler example, make that work, and then expand to the real situation you need the program for.

So, let's use a dummy DNA sequence first.


~~~
seq = 'A' * 10 + 'C' * 10 + 'G' * 10 + 'T' * 10
seq = 3 * seq
print(seq)
~~~
{: .language-python}

Now we split it into 10 base windows.
We can us the `range(start, stop, step)` command to construct a loop for the indices:


~~~
length = len(seq)
window = 10
for i in range(0, length, window):
    print(i)
~~~
{: .language-python}

This givs us the starting index. The final index is the starting index + the length of the window. Let's check we now get the right indices:

* from 0 to 10 (not including)
* from 10 to 20 (not including)
* from 20 to 30 (not including)
* etc


~~~
length = len(seq)
window = 10
for i in range(0, length, window):
    print(i, i + window)
~~~
{: .language-python}

Now we can slice the dna sequence:


~~~
length = len(seq)
window = 10
for i in range(0, length, window):
    print(i, i + window, seq[i: i + window])
~~~
{: .language-python}

What if we do 100 bp windows?


~~~
length = len(seq)
window = 100
for i in range(0, length, window):
    print(i, i + window, seq[i: i + window])
~~~
{: .language-python}

#### Creating non-overlapping windows, DNA

We have the basic code, let's adjust it for our genome. We'll test for the first 10 windows only here


~~~
length = 10000 # would normally use len(genome)
window = 100
for i in range(0, length, window):
    segment = genome[i: i + window]
    print(i, i + 10, segment)
~~~
{: .language-python}

#### GC percentage

Now we can start developing code for calculating the GC% for each segment. We have seen this before in one of the exercises:


~~~
dna = "ACGT"
g = dna.count("G")
c = dna.count("C")
gc = (g + c)*100/len(dna)
print(gc)
print(round(gc))
~~~
{: .language-python}

For simplicity, we will round off the percentage to whole integers.

#### Wrap this in a function


~~~
def perc_gc(dna):
    """Calculates percentage GC for a DNA sequence
       Rounds off to zero decimals"""
    dna = dna.upper()
    g = dna.count("G")
    c = dna.count("C")
    gc = (g + c)*100/len(dna)
    return(round(gc))

dna = "ACGT"
print(perc_gc(dna))
~~~
{: .language-python}

#### Use our function for the genome


~~~
length = len(genome)
window = 100
for i in range(0, length, window):
    segment = genome[i: i + window]
    print(perc_gc(segment))
~~~
{: .language-python}

We want to collect the percentages so we can plot a histogram og their frequencies. We'll try a list of percentages first.


~~~
length = len(genome)
window = 100
GCs = []
for i in range(0, length, window):
    segment = genome[i: i + window]
    gc = perc_gc(segment)
    GCs.append(gc)
~~~
{: .language-python}

Let's plot this! We use the `hist` (histogram) function from `matplotlib`:


~~~
import matplotlib.pyplot as plt
plt.hist(GCs)
plt.show()
~~~
{: .language-python}

With more resolution:


~~~
plt.hist(GCs, bins = 100)
plt.show()
~~~
{: .language-python}

#### Use dictionary instead

Here is a way to make a line plot of the frequencies. There are better ways, but this illustrates the principle with fairly easy code. First, we replace the list with a dictionary:


~~~
length = len(genome)
window = 100
GCs = {}
for i in range(0, length, window):
    segment = genome[i: i + window]
    gc = perc_gc(segment)
    GCs[gc] = GCs[gc] + 1
~~~
{: .language-python}

Hmm, this does not work as the dictionary is empty and we try to increase a non exitsing key + value combination.

We can set up the dictionary to have all possible keys (gc percentages), with `0` for all values:


~~~
GCs = {}
for i in range(0,100):
    GCs[i] = 0
GCs
~~~
{: .language-python}

Now we can fill the dictionary:


~~~
for i in range(0, length, window):
    segment = genome[i: i + window]
    gc = perc_gc(segment)
    GCs[gc] += 1
~~~
{: .language-python}

The full program than becomes:


~~~
length = len(genome)
window = 100
GCs = {}
for i in range(0,100):
    GCs[i] = 0
for i in range(0, length, window):
    segment = genome[i: i + window]
    gc = perc_gc(segment)
    GCs[gc] += 1
~~~
{: .language-python}

~~~
GCs
~~~
{: .language-python}

#### Plotting

Now we can make a line plot with keys on the `x` axis and values on the `y`:


~~~
plt.plot(GCs.keys(), GCs.values(), '-')
plt.show()
~~~
{: .language-python}

Adding axis labels:


~~~
plt.plot(GCs.keys(), GCs.values(), '-')
plt.xlabel("Percentage GC in 100 bp window")
plt.ylabel("Count")
plt.show()
~~~
{: .language-python}

#### Wrap all parts in a function


~~~
def get_genome(filename):
    """Reads a fasta file and splits into identifier (first line)
       and sequence (concatenation of all other lines)"""
    f = open(filename)
    lines = f.readlines()
    f.close()

    identifier = lines[0]
    genome = ""
    for line in lines[1:]:
        genome += line.strip()
    return(identifier, genome)

filename = 'data/NC_000913_E_coli_K12.fasta'
identifier, genome = get_genome(filename)
print(identifier, len(genome))
~~~
{: .language-python}

~~~
def get_gc_freq(genome, window):
    """Creates a dictionary with frequency of %GC for each window in genome"""
    length = len(genome)
    GCs = {}
    for i in range(0,100):
        GCs[i] = 0
    for i in range(0, length, window):
        segment = genome[i: i + window]
        gc = perc_gc(segment)
        GCs[gc] += 1
    return(GCs)
~~~
{: .language-python}

The main program becomes now very short!


~~~
# load genome
filename = 'data/NC_000913_E_coli_K12.fasta'
identifier, genome = get_genome(filename)
print(identifier, len(genome))

# get GC frequencies
window = 100
GCs = get_gc_freq(genome, window)

# plot
plt.plot(GCs.keys(), GCs.values(), '-')
plt.xlabel("Percentage GC in 100 bp window")
plt.ylabel("Count")
plt.show()
~~~
{: .language-python}

Let's try the two other genomes in the `data` folder:


~~~
filename2 = 'data/AKVW01.1_Rhodobacter_sphaeroides.fasta'
id2, genome2 = get_genome(filename2)
print(id2, len(genome2))
GCs2 = get_gc_freq(genome2, window)
plt.plot(GCs2.keys(), GCs2.values(), '-')
plt.xlabel("Percantage GC in 100 bp window")
plt.ylabel("Count")
plt.show()
~~~
{: .language-python}

Can we plot two datasets together?


~~~
plt.plot(GCs.keys(), GCs.values(), '-')
plt.plot(GCs2.keys(), GCs2.values(), '-')
plt.xlabel("Percantage GC in 100 bp window")
plt.ylabel("Count")
plt.show()
~~~
{: .language-python}

What about the third dataset?


~~~
filename3 = 'data/AL844501.2_Plasmodium_falciparum.fasta'
id3, genome3 = get_genome(filename3)
print(id3, len(genome3))
GCs3 = get_gc_freq(genome3, window)
plt.plot(GCs.keys(), GCs.values(), '-')
plt.plot(GCs2.keys(), GCs2.values(), '-')
plt.plot(GCs3.keys(), GCs3.values(), '-')
plt.xlabel("Percantage GC in 100 bp window")
plt.ylabel("Count")
plt.show()
~~~
{: .language-python}

#### Scaling the Malaria data


~~~
GCs3 = get_gc_freq(genome3[0:7000000], window)
plt.plot(GCs.keys(), GCs.values(), '-')
plt.plot(GCs2.keys(), GCs2.values(), '-')
plt.plot(GCs3.keys(), GCs3.values(), '-')
plt.xlabel("Percantage GC in 100 bp window")
plt.ylabel("Count")
plt.show()
~~~
{: .language-python}

#### Add legend


~~~
GCs3 = get_gc_freq(genome3[0:7000000], window)
plt.plot(GCs.keys(), GCs.values(), '-', label = "E. coli")
plt.plot(GCs2.keys(), GCs2.values(), '-', label = "R. sphaeroides")
plt.plot(GCs3.keys(), GCs3.values(), '-', label = "P. falciparum")
plt.xlabel("Percantage GC in 100 bp window")
plt.ylabel("Count")
plt.legend()
plt.show()
~~~
{: .language-python}


## Many styles of plot are available.

*   For example, using a fancier style.

~~~
plt.style.use('ggplot')
plt.plot(GCs.keys(), GCs.values(), '-', label = "E. coli")
plt.plot(GCs2.keys(), GCs2.values(), '-', label = "R. sphaeroides")
plt.plot(GCs3.keys(), GCs3.values(), '-', label = "P. falciparum")
plt.xlabel("Percantage GC in 100 bp window")
plt.ylabel("Count")
plt.legend()
plt.show()
~~~
{: .language-python}

**The data**

The references used for alignment were
* *E. coli K12 substr. MG1655* [GenBank NC_000913.2](https://www.ncbi.nlm.nih.gov/nuccore/NC_000913.2)
* *R. sphaeroides 2.4.1* chromosome 1 [GenBank AKVW01000000](https://www.ncbi.nlm.nih.gov/nuccore/AKVW01000001.1)
* *P. falciparum* (Malaria parasite) chromosome 1, [GenBank LR131290.1](https://www.ncbi.nlm.nih.gov/nuccore/LR131290.1)




<!--
> ## Minima and Maxima
>
> Fill in the blanks below to plot the minimum GDP per capita over time
> for all the countries in Europe.
> Modify it again to plot the maximum GDP per capita over time for Europe.
>
> ~~~
> data_europe = pandas.read_csv('data/gapminder_gdp_europe.csv', index_col='country')
> data_europe.____.plot(label='min')
> data_europe.____
> plt.legend(loc='best')
> plt.xticks(rotation=90)
> ~~~
> {: .language-python}
>
> > ## Solution
> >
> > ~~~
> > data_europe = pandas.read_csv('data/gapminder_gdp_europe.csv', index_col='country')
> > data_europe.min().plot(label='min')
> > data_europe.max().plot(label='max')
> > plt.legend(loc='best')
> > plt.xticks(rotation=90)
> > ~~~
> > {: .language-python}
> > ![Minima Maxima Solution](../fig/9_minima_maxima_solution.png)
> {: .solution}
{: .challenge}

> ## Correlations
>
> Modify the example in the notes to create a scatter plot showing
> the relationship between the minimum and maximum GDP per capita
> among the countries in Asia for each year in the data set.
> What relationship do you see (if any)?
>
> ~~~
> data_asia = pandas.read_csv('data/gapminder_gdp_asia.csv', index_col='country')
> data_asia.describe().T.plot(kind='scatter', x='min', y='max')
> ~~~
> {: .language-python}
>
> > ## Solution
> >
> > ![Correlations Solution 1](../fig/9_correlations_solution1.svg)
> >
> > No particular correlations can be seen between the minimum and maximum gdp values
> > year on year. It seems the fortunes of asian countries do not rise and fall together.
> >
> {: .solution}
>
> You might note that the variability in the maximum is much higher than
> that of the minimum.  Take a look at the maximum and the max indexes:
>
> ~~~
> data_asia = pandas.read_csv('data/gapminder_gdp_asia.csv', index_col='country')
> data_asia.max().plot()
> print(data_asia.idxmax())
> print(data_asia.idxmin())
> ~~~
> {: .language-python}
> > ## Solution
> > ![Correlations Solution 2](../fig/9_correlations_solution2.png)
> >
> > Seems the variability in this value is due to a sharp drop after 1972.
> > Some geopolitics at play perhaps? Given the dominance of oil producing countries,
> > maybe the Brent crude index would make an interesting comparison?
> > Whilst Myanmar consistently has the lowest gdp, the highest gdb nation has varied
> > more notably.
> >
> {: .solution}
{: .challenge}

> ## More Correlations
>
> This short program creates a plot showing
> the correlation between GDP and life expectancy for 2007,
> normalizing marker size by population:
>
> ~~~
> data_all = pandas.read_csv('data/gapminder_all.csv', index_col='country')
> data_all.plot(kind='scatter', x='gdpPercap_2007', y='lifeExp_2007',
>               s=data_all['pop_2007']/1e6)
> ~~~
> {: .language-python}
>
> Using online help and other resources,
> explain what each argument to `plot` does.
>
> > ## Solution
> > ![More Correlations Solution](../fig/9_more_correlations_solution.svg)
> >
> > A good place to look is the documentation for the plot function -
> > help(data_all.plot).
> >
> > kind - As seen already this determines the kind of plot to be drawn.
> >
> > x and y - A column name or index that determines what data will be
> > placed on the x and y axes of the plot
> >
> > s - Details for this can be found in the documentation of plt.scatter.
> > A single number or one value for each data point. Determines the size
> > of the plotted points.
> >
> {: .solution}
{: .challenge}
-->

> ## Saving your plot to a file
>
> If you are satisfied with the plot you see you may want to save it to a file,
> perhaps to include it in a publication. There is a function in the
> matplotlib.pyplot module that accomplishes this:
> [savefig](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.savefig.html).
> Calling this function, e.g. with
> ~~~
> plt.savefig('my_figure.png')
> ~~~
> {: .language-python}
>
> will save the current figure to the file `my_figure.png`. The file format
> will automatically be deduced from the file name extension (other formats
> are pdf, ps, eps and svg).
>
> Note that functions in `plt` refer to a global figure variable
> and after a figure has been displayed to the screen (e.g. with `plt.show`)
> matplotlib will make this  variable refer to a new empty figure.
> Therefore, make sure you call `plt.savefig` before the plot is displayed to
> the screen, otherwise you may find a file with an empty plot.
>

<!--
> When using dataframes, data is often generated and plotted to screen in one line,
> and `plt.savefig` seems not to be a possible approach.
> One possibility to save the figure to file is then to
>
> * save a reference to the current figure in a local variable (with `plt.gcf`)
> * call the `savefig` class method from that varible.
>
> ~~~
> fig = plt.gcf() # get current figure
> data.plot(kind='bar')
> fig.savefig('my_figure.png')
> ~~~
> {: .language-python}
-->

{: .callout}

> ## Making your plots accessible
>
> Whenever you are generating plots to go into a paper or a presentation, there are a few things you can do to make sure that everyone can understand your plots.
> * Always make sure your text is large enough to read. Use the `fontsize` parameter in `xlabel`, `ylabel`, `title`, and `legend`, and [`tick_params` with `labelsize`](https://matplotlib.org/2.1.1/api/_as_gen/matplotlib.pyplot.tick_params.html) to increase the text size of the numbers on your axes.
> * Similarly, you should make your graph elements easy to see. Use `s` to increase the size of your scatterplot markers and `linewidth` to increase the sizes of your plot lines.
> * Using color (and nothing else) to distinguish between different plot elements will make your plots unreadable to anyone who is colorblind, or who happens to have a black-and-white office printer. For lines, the `linestyle` parameter lets you use different types of lines. For scatterplots, `marker` lets you change the shape of your points. If you're unsure about your colors, you can use [Coblis](https://www.color-blindness.com/coblis-color-blindness-simulator/) or [Color Oracle](https://colororacle.org/) to simulate what your plots would look like to those with colorblindness.
{: .callout}
