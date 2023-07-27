# Langchain for LLM development

Notes on the short course from [DeepLearning.AI][1]

## General

* [LangChain][2] (LC) provides abstraction for interacting with LLM APIs, i.e. [prompt templates and output parsers][3]
* LLMs are stateless, but when building a chat interface, we can supply the entire conversation as
context
* Embeddings create numerical representation for pieces of text covering their content/meaning - text with similar
content will have similar vectors
* Vector databases contain embedding vectors and the corresponding chunk of text (which are usually smaller than
the original document/text)
* Querying a vector database means creating an embedding for the search query and retrieving n most-similar chunks of text

## Core functionality

* Check out [prompts for common operations][4]
* Chain-of-Thought Reasoning (ReAct)
* [Chains][5] and [memory][6] (especially [ConversationSummaryBufferMemory][7]) lend themselves to building chat interfaces
* Use chains to use (multiple) output from one prompt as input to another prompt
* Memory can also be [stored and retrieved][8] in/from external databases, i.e. [vector databases][9]
* LangChain also supports developing Q&A applications over documents


[1]: https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/
[2]: https://python.langchain.com/docs/get_started/introduction.html
[3]: https://python.langchain.com/docs/modules/model_io/
[4]: https://ai.googleblog.com/2022/11/react-synergizing-reasoning-and-acting.html
[5]: https://python.langchain.com/docs/modules/chains/
[6]: https://python.langchain.com/docs/modules/memory/
[7]: https://python.langchain.com/docs/modules/memory/summary
[8]: https://python.langchain.com/docs/modules/memory/vectorstore_retriever_memory
[9]: https://python.langchain.com/docs/integrations/vectorstores/