# [Auditing the Partisanship of Google Search Snippets](https://cbw.sh/static/pdf/hu-www19.pdf)

## Introduction
Google is the most used fact-checking tool.

Different types of quereis often return different types of results (e.g. tweets vs. news articles), these can have different qualities.

Snippets are summarizations of webpages shown in grey-ish on Google and can influence people without them clicking on the adjacent link.

It is suspected that Google did not engineer this, but the authors of the articles use partisan wording in response to speeches from politicians.

## Background
Automated snippet generation is a variant of extractive summarization instead of abstractive summarization.

Different parties use different words *cues* for the same entity.

## Dataset and Methodology
### Partisan Clues
Partisan cues are modeled as unigrams as bigrams and are derived much like by [Gentzkow and Shapiro](https://web.stanford.edu/~gentzkow/research/biasmeas.pdf).
Which give each n-gram a partisanship based on their frequency they have been used by the politicians of a party. Instead of speeches directly they use Vote Smart.

Old documents pollute the dataset, as those may not be relevant for the party and or the search engine. Cleaning and Tokenization with NLTK, discard too few used.

Partisanship(phrase) = (Proba_rep(phrase)-Proba_dem(phrase))/(Prob_rep(phrase)+Prob_dem(phrase))

The final histogram is rather well balanced.

### Obtaining SERPs
Query for US Politicians and partisan bigrams from the lexicon and their auto-completes. Some bigrams had to be completed/discarded by labelers.

The non-image/maps/google links were followed and the text between the <page>-tags was collected.

### Scoring SERPs
Clean and tokenize with NLTK. Remove query terms, as they are too partisan(?). The score of the whole document is the score of the n-grams divided by the amount of all (partisan and non-partisan) n-grams in the document.

Relative Bias= bias_query-bias_page if bias_page>0, else bias_page-bias_query.

In the final analysis only pages and SERPs were taken if they both have at least one strong n-gram, that is in the top x% of all n-grams.

## Analysis
Longer text is more evened out, compared to SERPs, as they have more n-grams. Spearman used to test for correlation. Shorter text amplified the partisanship half the time, in one fourth it flipped the partisanship.

The summarizer prioritizes the beginning of documents, where partisan clues are more concentrated. Clustered queries by topic (made by hand by them) to show results. Give bar-charts for queries based in cluster + polarity.

Summarization performs differently based on the site they want to summarize. Visual meta-data makes no difference on the polarizing behavior of the SERP, but invisible (e.g.description, image text) makes a difference in that the SERPs are now less amplifying.

## Conclusion
Summarization makes an impact on the bias of texts and it should be tested or made more transparent by Google.

The samples do not necessarily allign with other measurements of the websites slant. Also the score for partisanship is only a (subjective) model and not undiscussably set in stone.