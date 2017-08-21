# Introduction

## Basic

**Title**: [TextRank: Bringing Order into Texts](https://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf)

**Author**: Rada Mihalcea and Paul Tarau

**Publish information**: 2004, *Association for Computational Linguistics*

## Application scenario
Keyword / keyphrase / key sentence (summarization) extraction

# Model
TextRank: a graph-based ranking model.

## Keyword extraction

**Undirected and unweighted graph** (out-degree of a vertex == in-degree of this vertex)

**Vertex**: words (or specific part-of-speech words, such as nouns and adjectives)

**Edge**: two words are connected if they co-occur within a size fixed window

**Word score formula**:

![ ](https://github.com/gaoisbest/Paper_notes/blob/master/SML_1_2004_TextRank%20Bringing%20Order%20into%20Texts/Formula%201_keyword%20score.png)

```
A larger window does not
seem to help – on the contrary, the larger the window,
the lower the precision, probably explained by
the fact that a relation between words that are further
apart is not strong enough to define a connection in
the text graph.
```

## Keyphrase extraction
Adjacent keywords extracted by above method are collapsed into a keyphrase.



## Key sentence (summarization) extraction:

**Undirected and weighted graph**

**Vertex**: sentences

**Edge**: content overlapping 'similarity' relation, which can includes specific part-of-speech words (such as nouns and adjectives)

**Sentence score formula**:

**Similarity formula**:

denominator means normalization, which remove the effects of sentence length.


## Implementation

step 1: words tokenizing (segmentation)
step 2: part-of-speech annotating (this step is perfered to obtain better results)
step 3: graph construction 
step 4: iterate until converge (threshold is 0.0001)
step 5: return top K words with highest scores

# My comments
TextRank utilize the global information of the context, which likes the principle of GloVe.

In **Discussion** of **Keyword Extraction**, the paper said that 
```
Regardless of the direction chosen for the arcs, results obtained
with directed graphs are worse than results obtained
with undirected graphs, which suggests that
despite a natural flow in running text, there is no natural
“direction” that can be established between co-occurring words.
```
, which showed that **no order information** is considered within the window. It is similarity like the Skip-gram model in word2vec.

However, this may conflict with the paper title 'Bringing **Order** into Texts'.

# Application

