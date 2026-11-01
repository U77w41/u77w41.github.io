---
title: Analysis on Complete Works of Swami Vivekananda
slug: analysis-on-complete-works-of-swami-vivekananda
description: Estimating vocabulary cardinality in Swami Vivekananda's complete works using the Flajolet-Martin streaming algorithm.
image: swami-ji.png
date: 2023-12-08T00:00:00+00:00
weight: 8
categories:
- Data Mining
tags:
- Text Analysis
- Text Mining
- Data Mining
- Algorithms
- Python
---

**[GitHub Repository](https://github.com/U77w41/Analysis-on-Complete-Works-of-Swami-Vivekananda/)**

## Overview

Calculating the exact count of unique items in a large data stream typically requires maintaining an in-memory hash table of every distinct entry seen so far. As the corpus grows into millions of tokens, memory consumption scales linearly ($O(N)$), creating bottlenecks in streaming and distributed pipelines.

The **Flajolet-Martin (FM) algorithm** solves this by probabilistically estimating stream cardinality using a fixed, minimal amount of memory ($O(\log N)$ bits). This project applies the Flajolet-Martin algorithm to estimate the unique vocabulary size across the nine volumes of the *Complete Works of Swami Vivekananda*, comparing the probabilistic estimate against exact set cardinality.

## The Flajolet-Martin Algorithm

The algorithm estimates distinct elements by tracking the distribution of trailing zeros in hashed bit representations:

1. **Hash Mapping:** Each token $w$ in the text stream is hashed to a uniform 32-bit or 64-bit integer $h(w)$.
2. **Trailing Zeros ($\rho$):** For each hash value, the algorithm determines $\rho(h(w))$—the number of trailing zeros before the first 1-bit. A hash ending in $k$ zeros occurs with probability $2^{-(k+1)}$.
3. **Maximum Tracking ($R$):** The algorithm maintains $R = \max(\rho(h(w)))$ across all processed tokens. If we observe a sequence ending in $R$ trailing zeros, we can infer that roughly $2^R$ unique elements have passed through the stream.
4. **Correction Factor:** The raw estimate is adjusted using the Flajolet-Martin correction factor $\phi \approx 0.77351$:

$$\text{Estimated Cardinality} \approx \frac{2^R}{\phi}$$

### Variance Reduction (Stochastic Averaging)

A single hash function has significant statistical variance. To produce a stable estimate:

* The stream is evaluated across $m$ independent hash functions.
* The hash functions are divided into $g$ distinct groups.
* The arithmetic mean of estimates is computed within each group.
* The overall result is obtained by taking the **median of group averages**, filtering out extreme outlier hash values.

## Processing Pipeline

The text processing pipeline handles the multi-volume corpus in Python:

1. **Text Extraction & Cleaning:** Reads raw volume texts, stripping metadata, page numbers, and punctuation while normalizing text to lowercase tokens.
2. **Token Streaming:** Simulates a continuous token stream rather than holding the entire preprocessed vocabulary in memory.
3. **Multi-Hash Simulation:** Generates independent hash functions using linear congruential generators:

$$h_i(x) = (a_i \cdot x + b_i) \pmod p$$

4. **Benchmarking:** Compares the FM probabilistic cardinality against standard Python `set()` counting to evaluate error bounds across different numbers of hash buckets.

## Key Insights

* **Memory Efficiency:** While exact counting requires storing hundreds of thousands of unique string tokens in memory, the Flajolet-Martin register array requires only a few bytes of storage.
* **Accuracy Trade-off:** Increasing the number of hash functions significantly tightens the standard error, bringing the estimated vocabulary count within 5-10% of the true distinct word count.
