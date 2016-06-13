---
layout: post
title:  "5 variants of word embeddings"
date:   2016-05-31 11:15:12 +0530
categories: nlp tools opensource machinelearning
---

[Word Embdeddings](https://en.wikipedia.org/wiki/Word_embedding) are extremely interesting and exciting. The fact that you can now represent words in vectors space opens up a lot of avenues on how Natural Language Processing will be done.

<img src="/assets/images/5-variants-of-word-embdeddings/word-embedding.png" />

In original paper [Distributed Representations of Words and Phrases and their Compositionality](https://arxiv.org/abs/1310.4546) Mikolov et. al described Skip-gram model. Which given a word tries to predict the context around that word. Inverse of Skip-gram model is Continuous Bag-Of-Word model which given the context tries to predict the word. CBOW is described in more details in [Efficient Estimation of Word Representations in Vector Space](http://arxiv.org/pdf/1301.3781.pdf) .

Following is a compilation of 5 variants of the original [word2vec](https://en.wikipedia.org/wiki/Word2vec) class of models

1. [doc2vec](https://radimrehurek.com/gensim/models/doc2vec.html) although not officially named in paper [Distributed Representations of Sentences and Documents](https://cs.stanford.edu/~quocle/paragraph_vector.pdf) it an embedding of documents from documents to vector space. Possible applications include Classification and Clustering as with any other lexical units.

2. [tweet2vec](https://arxiv.org/abs/1605.03481) this paper talks about embedding tweets into vector space. This is situations where you have lot of noisy text and informal language structure.

3. [item2vec](http://arxiv.org/abs/1603.04259), dealing with item and user similarity is at heart of lot of recommendation algorithms. Item2vec talks about embedding users/items into vector space representation. This is similar to SVD or other Matrix Factorization algorithms which generate low-rank matrix as approximation of original rating matrix.

4. [lda2vec](https://github.com/cemoody/lda2vec), this embedding technique tries to marry best of both worlds, word2vec and LDA. I can see this having a lot of values in domain where there are mixed topics in text.

5. [Wikipedia Navigation Vectors](https://meta.wikimedia.org/wiki/Research:Wikipedia_Navigation_Vectors), Wikipedia is my preferred resource for NLP tasks. This research project take an interesting idea of modelling co-visitation in session and try to figure out similar articles based on raw logs. Be sure to check out the visualization on the page, it's really interesting.

6. [search2vec](https://research.yahoo.com/publications/8758/scalable-semantic-matching-queries-ads-sponsored-search-advertising), Broad matching is big deal in online advertising. Historically this has been done using query expansion and rewriting. This paper talks about embedding search logs's action along with ad information. Key highlight of this paper is that it talks about doing distributed embedding using Hadoop. 
