# Large Language Models with Semantic Search

Notes on the short course from [DeepLearning.AI][1]

## General

* Search consists of retrieval and re-ranking stages

## Keyword Search

* Keyword search: The more words the query and the document share, the higher the score
* [BM25][2] is a core algorithm used for retrieval stage in keyword search
* Language models help with limitations of keyword search

## Dense Retrieval

* Dense retrieval is one of the two main methods to do semantic search (the other one is ReRank)
* It finds relevant results by retrieving items which are closest (approximate nearest neighbors) to the search query
* Depending on the model used to generate embeddings, search queries can be crafted in different language and still yield the same results

## ReRank

* For each query/response pair, ReRank calculates a relevance score and selects items with the highest score as results

[1]: https://www.deeplearning.ai/short-courses/large-language-models-semantic-search/
[2]: https://en.wikipedia.org/wiki/Okapi_BM25