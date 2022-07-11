# Markov Chain
This repository contains three applications of Markov chain state machines written in C. A Markov chain is a state machine, typically depicted as a directed graph, where the edges between two states are probabilities (ranging between 0-1). The transitions within the directed graph can be modeled as a 2 dimensional array of probabilities, where the indices to the 2d array denote the transition between 2 states (i.e., `transition_matrix[FROM][TO]`). Traversing the graph, while following and adhering to the calculated probabilities generates results in a procedural manner. 

The `word_generator.c` file is the simplest markov chain providing a way to generate pseudo-random words by analyzing a list of existing words. Through analysis of the provided dataset, probability weights are calculated for the states of every alphabetic letter (a-z) and their transitions to other letters. These are then used to generate words. 

The `sentence_generator.c` file provides a way to generate semi-coherent sentences by analyzing text. Again, probability weights are calculated for each word, rather than letter, and their transitions to other words. 

Notably, both the **word** and **sentence** chains only look at the current letter and word, respectively, in the sequence of the word and sentence. The problem with this approach is that an arbitrary state has little information about the coherency of the overarching structure (i.e., the entire word or the entire sentence). To solve for this, n-grams can be used to pack several tokens together as states. The `ngram_word_generator.c` provides a way of generating words where an extra parameter determines the size of n-grams used. 

The datasets folder contains example datasets to be used when running the executable. Note that the datasets containing single words per line are meant for the word generator(s), and datasets containing full texts are meant for the sentence generator. The project uses a Makefile as build tool.

## Quick start word generator
---
```
$ git clone https://github.com/Janko-dev/Markov_Chain.git
$ make word
$ ./wgen -d <filepath> <num_generated_words>
```
- `-d` flag is an optional flag that enables debug output (like printing the calculated transition matrix)
- `<filepath>` is the file path parameter that is used to read the training data for the model. Provide a correct filepath that contains a list of words seperated by **line breaks**.
- `<num_generated_words>` is the number of words that will be generated by the model. 

## Quick start sentence generator
---
```
$ git clone https://github.com/Janko-dev/Markov_Chain.git
$ make sent
$ ./sgen -d <filepath> <num_generated_sentences>
```
- `-d` flag is an optional flag that enables debug output (like printing the calculated transition matrix)
- `<filepath>` is the file path parameter that is used to read the training data for the model. Provide a correct filepath that contains a text of data to be parsed.
- `<num_generated_sentences>` is the number of sentences that will be generated by the model. 

## Quick start n-gram word generator
---
```
$ git clone https://github.com/Janko-dev/Markov_Chain.git
$ make ngram_word
$ ./ngram -d <filepath> <num_generated_words> <n> <max_length>
```
- `-d` flag is an optional flag that enables debug output of the params
- `<filepath>` is the file path parameter that is used to read the training data for the model. Provide a correct filepath that contains a text of data to be parsed.
- `<num_generated_words>` is the number of words that will be generated by the model. 
- `<n>` is the size of the n-gram tokens (e.g., the string "laptop" with n = 2 would result in the following tokens: "la", "pt", "op")
- `<max_length>` is the maximum length that a generated word can take. Using multiples of `<n>` is preferred. 

## Example: username generator
Fun way to generate a username based on a Markov chain that was trained on all pokemon names. Consider the difference in using the word generator and the n-gram word generator. 
```
$ make word && ./wgen -d datasets/pokemon_names.txt 10
gcc word_generator.c -o wgen -Wall -Wextra -g  

--------------------------------------------------
Filepath:               datasets/pokemon_names.txt
# of generated words:   10
Debug mode:             true
--------------------------------------------------
   a    b    c    d    e    f    g    h    i    j    k    l    m    n    o    p    q    r    s    t    u    v    w    x    y    z 
a  0.00 0.03 0.03 0.03 0.00 0.01 0.04 0.00 0.03 0.00 0.02 0.05 0.04 0.13 0.00 0.02 0.00 0.35 0.06 0.06 0.02 0.02 0.02 0.01 0.01 0.01
b  0.17 0.04 0.01 0.00 0.10 0.00 0.00 0.00 0.09 0.00 0.00 0.10 0.00 0.01 0.09 0.00 0.00 0.17 0.02 0.00 0.17 0.00 0.00 0.00 0.05 0.00
c  0.12 0.00 0.02 0.00 0.05 0.00 0.00 0.24 0.03 0.00 0.07 0.07 0.00 0.01 0.09 0.00 0.00 0.20 0.00 0.07 0.03 0.00 0.00 0.00 0.01 0.00
d  0.06 0.00 0.00 0.03 0.15 0.00 0.02 0.00 0.13 0.00 0.01 0.02 0.00 0.00 0.20 0.00 0.00 0.25 0.01 0.00 0.10 0.00 0.01 0.00 0.01 0.00
e  0.07 0.02 0.03 0.05 0.08 0.01 0.02 0.00 0.02 0.00 0.02 0.14 0.02 0.08 0.06 0.02 0.00 0.17 0.04 0.07 0.01 0.01 0.02 0.01 0.02 0.01
f  0.11 0.02 0.00 0.00 0.18 0.17 0.00 0.00 0.06 0.00 0.02 0.21 0.00 0.00 0.06 0.00 0.00 0.11 0.00 0.03 0.03 0.00 0.00 0.00 0.02 0.00
g  0.13 0.01 0.01 0.01 0.10 0.00 0.06 0.01 0.08 0.00 0.00 0.08 0.02 0.02 0.18 0.00 0.00 0.22 0.01 0.01 0.05 0.00 0.00 0.00 0.02 0.01 
h  0.18 0.00 0.00 0.00 0.14 0.00 0.00 0.00 0.18 0.00 0.01 0.02 0.00 0.00 0.13 0.00 0.00 0.23 0.00 0.02 0.05 0.00 0.00 0.00 0.04 0.00
i  0.04 0.01 0.07 0.04 0.03 0.01 0.06 0.00 0.00 0.00 0.02 0.10 0.03 0.16 0.05 0.04 0.00 0.12 0.07 0.09 0.01 0.01 0.01 0.01 0.00 0.01
j  0.04 0.00 0.00 0.00 0.01 0.00 0.00 0.00 0.02 0.00 0.00 0.00 0.00 0.00 0.02 0.00 0.00 0.88 0.00 0.00 0.01 0.00 0.00 0.00 0.01 0.00
k  0.12 0.00 0.00 0.00 0.10 0.00 0.00 0.01 0.17 0.00 0.00 0.06 0.00 0.01 0.07 0.00 0.00 0.38 0.00 0.02 0.03 0.00 0.00 0.00 0.03 0.00
l  0.14 0.02 0.01 0.03 0.20 0.01 0.02 0.00 0.16 0.00 0.00 0.09 0.01 0.00 0.11 0.02 0.00 0.05 0.00 0.03 0.07 0.01 0.00 0.00 0.03 0.00
m  0.31 0.05 0.01 0.00 0.19 0.00 0.00 0.00 0.15 0.00 0.00 0.00 0.00 0.00 0.12 0.10 0.00 0.01 0.01 0.00 0.05 0.00 0.00 0.00 0.01 0.00
n  0.08 0.01 0.04 0.04 0.11 0.02 0.09 0.00 0.09 0.01 0.04 0.00 0.01 0.03 0.07 0.01 0.00 0.20 0.03 0.08 0.02 0.00 0.00 0.01 0.02 0.01
o  0.03 0.01 0.02 0.03 0.01 0.01 0.02 0.00 0.02 0.00 0.02 0.07 0.05 0.16 0.07 0.03 0.00 0.23 0.06 0.06 0.04 0.01 0.05 0.01 0.00 0.00
p  0.09 0.01 0.00 0.00 0.10 0.00 0.00 0.05 0.16 0.00 0.00 0.04 0.00 0.00 0.11 0.03 0.00 0.28 0.02 0.02 0.05 0.00 0.00 0.00 0.01 0.00
q  0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.52 0.00 0.00 0.43 0.00 0.04 0.00 0.00 0.00
r  0.19 0.02 0.02 0.03 0.11 0.01 0.01 0.01 0.12 0.00 0.01 0.03 0.03 0.03 0.17 0.02 0.00 0.04 0.02 0.03 0.05 0.01 0.00 0.00 0.03 0.00
s  0.06 0.01 0.06 0.00 0.12 0.00 0.00 0.13 0.05 0.00 0.07 0.05 0.02 0.03 0.02 0.06 0.01 0.05 0.06 0.11 0.03 0.00 0.05 0.00 0.01 0.00
t  0.16 0.00 0.02 0.00 0.11 0.00 0.00 0.08 0.10 0.00 0.00 0.03 0.02 0.00 0.14 0.00 0.00 0.13 0.00 0.07 0.05 0.00 0.01 0.00 0.05 0.02
u  0.01 0.03 0.03 0.04 0.02 0.03 0.03 0.00 0.03 0.00 0.00 0.04 0.06 0.13 0.01 0.03 0.00 0.32 0.10 0.04 0.00 0.00 0.00 0.02 0.00 0.01 
v  0.20 0.00 0.00 0.02 0.25 0.00 0.00 0.00 0.29 0.00 0.00 0.00 0.00 0.00 0.09 0.00 0.00 0.05 0.00 0.00 0.05 0.00 0.00 0.00 0.05 0.00
w  0.19 0.01 0.00 0.03 0.10 0.00 0.01 0.07 0.16 0.00 0.03 0.04 0.00 0.03 0.13 0.03 0.00 0.01 0.03 0.04 0.01 0.00 0.00 0.00 0.01 0.01
x  0.06 0.00 0.06 0.00 0.38 0.00 0.00 0.00 0.19 0.00 0.00 0.00 0.00 0.00 0.06 0.06 0.00 0.06 0.00 0.00 0.06 0.00 0.00 0.00 0.06 0.00
y  0.09 0.04 0.00 0.07 0.09 0.00 0.09 0.02 0.00 0.00 0.02 0.07 0.04 0.07 0.04 0.09 0.02 0.09 0.07 0.07 0.04 0.02 0.00 0.00 0.00 0.00
z  0.21 0.00 0.00 0.00 0.24 0.00 0.00 0.00 0.12 0.00 0.00 0.06 0.00 0.00 0.18 0.00 0.00 0.00 0.00 0.00 0.09 0.00 0.03 0.00 0.03 0.06

--------------------------------------------------
generated word  1:      cearspidr
generated word  2:      eliralee
generated word  3:      roskideeefeel
generated word  4:      wt
generated word  5:      randillareezo
generated word  6:      iotimealee
generated word  7:      pukri
generated word  8:      arinr
generated word  9:      yeoneormia
generated word 10:      idre
```
---
The following is the n-gram version with n = 2, and max length = 6
```
$ make ngram_word && ./ngram -d datasets/pokemon_names.txt 10 2 6
gcc ngram_word_generator.c -o ngram -Wall -Wextra -g  

-------------------------------------------------------------
Filepath:               datasets/pokemon_names.txt
# of generated words:   10
n-gram param:           2
Maximum size of words:  6
Debug mode:             true
---------------------- Generated words ----------------------
lkiado
yogole
zoroda
quilic
zapdos
wratty
xernea
pelibi
eeveno
pdosee
```
In testing both versions, the n-gram version gives more coherent words that don't look like gibberish.

## Inspiration
While studying for an exam on lineair algebra and stochastics, I came across queueing theory, which is yet another application of Markov chains. This led me to do research on Markov chains. 