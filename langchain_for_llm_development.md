# LangChain for LLM development

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
* Chain-of-Thought Reasoning (ReAct)

## Functionality

* Check out [prompts for common operations][4]
* [Chains][5] and [memory][6] (especially [ConversationSummaryBufferMemory][7]) lend themselves to building chat interfaces
* Use chains to use (multiple) output from one prompt as input to another prompt
* Memory can also be [stored and retrieved][8] in/from external databases, i.e. [vector databases][9]
* LangChain also supports developing [Q&A applications over documents][10]
* Use _QAGenerationChain_ to generate question/answer pairs automatically (using LLM)
* Use _QAEvalChain_ to evaluate question/answer pairs automatically (using LLM)
* Use `langchain.debug = True` to get debugging information
* [Agents][11] use LLMs as a reasoning engine and can use [tools][12], i.e. a calculator or searching information on Wikipedia, to complete tasks
* You can define your own tools by writing a Python function and decorating it with `@tool`


[1]: https://www.deeplearning.ai/short-courses/langchain-for-llm-application-development/
[2]: https://python.langchain.com/docs/get_started/introduction.html
[3]: https://python.langchain.com/docs/modules/model_io/
[4]: https://ai.googleblog.com/2022/11/react-synergizing-reasoning-and-acting.html
[5]: https://python.langchain.com/docs/modules/chains/
[6]: https://python.langchain.com/docs/modules/memory/
[7]: https://python.langchain.com/docs/modules/memory/summary
[8]: https://python.langchain.com/docs/modules/memory/vectorstore_retriever_memory
[9]: https://python.langchain.com/docs/integrations/vectorstores/
[10]: https://python.langchain.com/docs/use_cases/question_answering/
[11]: https://python.langchain.com/docs/modules/agents/
[12]: https://python.langchain.com/docs/integrations/tools/