# Text Mining 2: Word Embeddings & Dictionary-Based Sentiment Analysis

This repository contains an R Markdown project covering two text-mining
techniques: training word embeddings (GloVe) on the Harry Potter book series,
and performing dictionary-based sentiment analysis on IMDB movie reviews
using multiple sentiment lexicons, including the labMT dictionary.

## Overview

The project is split into two parts:

### 1. Word Embedding

Using the full texts of the first seven Harry Potter novels (via the
[`harrypotter`](https://github.com/bradleyboehmke/harrypotter) package), the
project builds a vocabulary, constructs a token co-occurrence matrix (TCM),
and trains 50-dimensional GloVe word vectors with `text2vec`. It then
explores the resulting embedding space by finding the most similar words to
key terms ("harry", "death", "love") via cosine similarity, and demonstrates
vector arithmetic (e.g. `harry + love - death`) to explore semantic
relationships captured by the embeddings.

### 2. Dictionary-Based Sentiment Analysis

Using the `movie_review` dataset (5,000 labeled IMDB reviews) from
`text2vec`, the project tokenizes and cleans the review text, then scores
sentiment using the **labMT dictionary** (Dodds et al., 2011) — a
word-happiness lexicon widely used in computational social science. Average
sentiment scores are computed per review, compared against the ground-truth
IMDB sentiment labels via a confusion matrix, and evaluated for accuracy —
both across the full dataset and restricted to the most extreme (top/bottom
25%) reviews, where prediction accuracy improves substantially.

## Contents

| Section | Description |
|---|---|
| **Load data set** | Load and reshape the seven Harry Potter books into a tidy data frame, one row per chapter |
| **Tokenize data frame** | Tokenize chapter text into individual words with `unnest_tokens` |
| **Remove stop words** | Filter out common function words via `anti_join(stop_words)` |
| **Vocabulary of unique terms** | Build a pruned vocabulary (min. 5 occurrences) with `text2vec::create_vocabulary` |
| **Token Co-occurrence Matrix (TCM)** | Construct a co-occurrence matrix using a context window of 5 |
| **Train word vectors** | Fit GloVe embeddings (rank 50, 20 iterations) on the TCM |
| **Combining word vectors** | Sum the main and context GloVe vector matrices for higher-quality embeddings |
| **Finding most similar words** | Use cosine similarity to find nearest neighbors for selected words |
| **Word vector manipulations** | Explore vector arithmetic on the trained embeddings |
| **Load dataset (sentiment)** | Load the `movie_review` IMDB dataset |
| **Lexicon overview** | Summarize `tidytext`'s four built-in lexicons (afinn, bing, nrc, loughran) and introduce the labMT dictionary |
| **Tokenization & stop word removal** | Prepare review text for sentiment scoring |
| **Finding sentiment scores** | Join tokens against the labMT dictionary to assign per-word sentiment values |
| **Average sentiment score per review** | Aggregate scores per review, identify the most positive/negative reviews, and visualize scores with a bar chart |
| **Evaluation** | Dichotomize average sentiment scores and compare against true labels via a confusion matrix |

## Key findings

- **Word embeddings:** The nearest neighbors to "harry" are dominated by
  other central characters and narrative context (ron, hermione, moment,
  time); "death" clusters with Death Eaters, Voldemort, and violence-related
  terms; "love" clusters more loosely with terms like potion(s) and
  wonderful — reflecting how the word is used contextually in the books.
  Vector arithmetic (`harry + love - death`) surfaces other core characters
  (ron, hermione, fred), showing the embedding space captures some
  meaningful relational structure.
- **Sentiment analysis:** Using the raw average labMT sentiment score as a
  classifier (threshold at 5.75) achieves **67.64% accuracy** against the
  true IMDB sentiment labels across all 5,000 reviews.
- Restricting evaluation to the top/bottom 25% most extreme average-sentiment
  reviews (removing likely-neutral reviews) improves accuracy to **77.0%**,
  showing the dictionary approach is considerably more reliable at the
  sentiment extremes than in ambiguous, near-neutral cases.

## Requirements

This project uses R with the following packages:

```r
install.packages(c("tidyverse", "tidytext", "text2vec", "remotes"))
remotes::install_github("bradleyboehmke/harrypotter")
```

- `tidyverse` — data wrangling and plotting (`dplyr`, `ggplot2`, `readr`, `purrr`)
- `tidytext` — tokenization and built-in sentiment lexicons
- `text2vec` — vocabulary building, co-occurrence matrices, GloVe training, and the `movie_review` dataset
- `harrypotter` — full texts of the Harry Potter novels (GitHub package, not on CRAN)

The labMT dictionary is downloaded directly from GitHub at knit time:
[`andyreagan/sentidict`](https://github.com/andyreagan/sentidict), based on
Dodds et al. (2011) — see the
[associated paper](https://link.springer.com/content/pdf/10.1140/epjds/s13688-017-0121-9.pdf).

## Repository structure

```
.
├── text-mining-2.Rmd     # R Markdown source
└── README.md
```

> **Note:** The `movie_review` dataset is bundled with `text2vec`, and the
> Harry Potter texts are loaded via the `harrypotter` package — no external
> data files are required beyond package installation and an internet
> connection (to fetch the labMT dictionary and install `harrypotter` from
> GitHub).

## Usage

Open `text-mining-2.Rmd` in RStudio and knit to PDF or HTML:

```r
rmarkdown::render("text-mining-2.Rmd")
```

## Author

Anthony Kamau
