[Lec01](C:/Users/YangHL/Desktop/Computer%20Science/AI/LLM/stanford_cs336/stanford_cs336_LLM_lec01.htm)
# Introduction
## Distance between small LLM and big LLM
### optimization directions
> 随着规模增大，FFN的参数量将达到超过$2/3$

![[Pasted image 20250714235417.png]]
### Scaling Phenomenon
![[Pasted image 20250714235617.png]]

## What can we learn
There are three types of knowledge:
1. **Mechanics**: how things work (what a Transformer is, how model parallelism leverages GPUs
2. **Mindset**: squeezing the most out of the hardware, taking scale seriously (scaling laws)
3. Intuitions: which data and modeling decisions yield good accuracy (for small LLM)

## What should we focus：efficiency of scalable algorithms
Wrong interpretation: scale is all that matters, algorithms don't matter.

Right interpretation: algorithms that scale is what matters.
```
accuracy = efficiency x resources
```
In fact, efficiency is way more important at larger scale (can't afford to be wasteful).
[Hernandez+ 2020](https://arxiv.org/abs/2005.04305) showed 44x algorithmic efficiency on ImageNet between 2012 and 2019

Framing: what is the best model one can build **given a certain compute and data budget?**

In other words, **maximize efficiency**!

## Current Landscape
### Neural ingredients (2010s)
- First neural language model [Bengio+ 2003](https://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf) 
- **Seq2Seq** modeling (for machine translation) [Sutskever+ 2014](https://arxiv.org/pdf/1409.3215.pdf)
- **Adam** optimizer [Kingma+ 2014](https://arxiv.org/pdf/1412.6980.pdf) 
- Attention mechanism (for machine translation) [Bahdanau+ 2014](https://arxiv.org/pdf/1409.0473.pdf) 
- **Transformer architecture** (for machine translation) [Vaswani+ 2017](https://arxiv.org/pdf/1706.03762.pdf) 
- Mixture of experts [Shazeer+ 2017](https://arxiv.org/pdf/1701.06538.pdf) 
- **Model parallelism** [Huang+ 2018](https://arxiv.org/pdf/1811.06965.pdf) [Rajbhandari+ 2019](https://arxiv.org/abs/1910.02054) [Shoeybi+ 2019](https://arxiv.org/pdf/1909.08053.pdf) 

### Openness
#openness 

## Overview
![[Pasted image 20250715152515.png]]

## Efficiency drives design decisions
Today, we are compute-constrained, so design decisions will reflect squeezing the most out of given hardware.
- **Data processing**: avoid wasting precious compute updating on bad / irrelevant data
- **Tokenization**: working with raw bytes is elegant, but compute-inefficient with today's model architectures.
- **Model architecture**: many changes motivated by reducing memory or FLOPs (e.g., sharing KV caches, sliding window attention)
- **Training**: we can get away with a single epoch!
- **Scaling laws**: use less compute on smaller models to do hyperparameter tuning
- **Alignment**: if tune model more to desired use cases, require smaller base models

# Tokenization
![[Pasted image 20250715161558.png]]

Intuition: break up string into popular segments
This course: **Byte-Pair Encoding (BPE) tokenizer** [Sennrich+ 2015](https://arxiv.org/abs/1508.07909) 
Tokenizer-free approaches:  [Xue+ 2021](https://arxiv.org/abs/2105.13626) [Yu+ 2023](https://arxiv.org/pdf/2305.07185.pdf) [Pagnoni+ 2024](https://arxiv.org/abs/2412.09871) [Deiseroth+ 2024](https://arxiv.org/abs/2406.19223) 
Use bytes directly, promising, but have not yet been scaled up to the frontier.

## Observations
- A word and **its preceding space** are part of the same token (e.g., " world").
- A word at the beginning and in the middle are represented differently (e.g., "hello hello").
- Numbers are tokenized into every few digits.

## Methods
### Character-based tokenization
> A Unicode string is a sequence of Unicode characters. Each character can be converted into a code point (integer) via `ord` in python.


1. Problem 1: this is a **very large vocabulary.**
2. Problem 2: many characters are quite rare (e.g., 🌍), which is inefficient use of the vocabulary.（cf. Thought in Huffman Code）

### Byte-based tokenization
> Unicode strings can be represented as a sequence of bytes, which can be represented by integers between 0 and 255.
> The most common Unicode encoding is [UTF-8](https://en.wikipedia.org/wiki/UTF-8) 

1. Advantage: The vocabulary is nice and small: a byte can represent 256 values.
2. **The compression ratio is terrible （=1）**, which means the sequences will be too long. Given that the context length of a Transformer is limited (since attention is quadratic), this is not looking great...

### Word-based tokenization
> Another approach (**closer to what was done classically in NLP**) is to split strings into words.

But there are problems:
- The **number of words is huge** (like for Unicode characters).
- Many words are rare and the model won't learn much about them.
- **This doesn't obviously provide a fixed vocabulary size**.

### Byte Pair Encoding (BPE)
> The BPE algorithm was introduced by Philip Gage in 1994 for data compression. [article](http://www.pennelynn.com/Documents/CUJ/HTML/94HTML/19940045.HTM) It was adapted to NLP for neural machine translation. [Sennrich+ 2015](https://arxiv.org/abs/1508.07909) (Previously, papers had been using word-based tokenization.) BPE was then used by GPT-2. [Radford+ 2019](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) 

#BPE 
Basic idea: _train_ the tokenizer ==on raw text to automatically determine the vocabulary.== Intuition: common sequences of characters are represented by a single token, rare sequences are represented by many tokens.

The GPT-2 paper used **word-based tokenization to break up the text into inital segments** and **run the original BPE algorithm on each segment**.

Sketch: start with each byte as a token, and successively merge the most common pair of adjacent tokens.
