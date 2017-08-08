---
layout: post
title: A Room for Jane Austen
summary: Exploration of ngrams and using ngram probabilities to construct sentences and paragraphs.
tags: [fun, profit, hadoop, java. scala]
author: ilan
---
## Day 0
### The Conjecture
Over the last year I have been thinking, reading and learning about
`words in documents`. And what kind of information can be extracted
from `words in documents`. One interesting avenue of exploration is:
- the probabilities of two words appearing next to each other in a
particular order. 
- The probability of three words appearing next to each other ... 
- And so on and so forth. 

Analysis of these probabilities have all sorts of interesting
implications; and, allow for fun experiments.

2017 is also a year for celebrating the novels of the great author
Jane Austen; as it has been 200 years since she passed on from this
world.

One way of participating in her celebration could be to create a Jane
Austen room, where using ngram probabilities from the words in her
novels, her corpus - one could interact with those probabilities in
various different ways using the game on protocols.

### The tooling.
- [Apache Hadoop](http://hadoop.apache.org/) is the data cruncher.
- [sample-scala-room](https://github.com/gameontext/sample-room-scala) is the base template for the room.
- [Project Gutenberg](https://www.gutenberg.org/) is the data source for the `words in documents` i.e. the novels of Jane Austen.
