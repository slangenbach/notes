# LangChain - Chat with your data

Notes on the short course from [DeepLearning.AI][1]

## General

* [Document loaders][2] transfer raw data into a standardized format
* You can use a speech-to-text model (e.g. [OpenAI's whisper model][3]) to load data from audi/video 
files
* Documents need to be split in order to be processed efficiently. Select a [splitter][4] and use
 `create_documents()` to create documents from a list of text and `split_documents()` to split - 
 `split_documents()` accepts custom regex expressions to split
* Store splits converted to embeddings in vector store (i.e. [Chroma][5] to get started) for 
efficient retrieval
* Start with [semantic similarity search][6] and try [maximum marginal relevance (MMR)][7],
[SelfQuery][8] (a.k.a. LLM aided retrieval) or [compression][9]
* The general retrieval workflow looks as follows:
    - User asks a question
    - Question is transformed to embedding, send to vector store and used to retrieve relevant splits
    - Relevant splits (system prompt) and original question (human prompt) and send to LLM
* Use [RetrievalQA chain][10] to ask questions about your documents and switch to 
[ConversationalRetrievalChain][11] and [ConversationBufferMemory][12] if you need to introduce memory


[1]: https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/
[2]: https://python.langchain.com/docs/modules/data_connection/document_loaders/
[3]: https://platform.openai.com/docs/guides/speech-to-text
[4]: https://python.langchain.com/docs/modules/data_connection/document_transformers/
[5]: https://github.com/chroma-core/chroma
[6]: https://python.langchain.com/docs/modules/model_io/prompts/example_selectors/similarity
[7]: https://python.langchain.com/docs/modules/model_io/prompts/example_selectors/mmr
[8]: https://python.langchain.com/docs/modules/data_connection/retrievers/self_query/
[9]: https://python.langchain.com/docs/modules/data_connection/retrievers/contextual_compression/
[10]: https://python.langchain.com/docs/use_cases/question_answering/how_to/vector_db_qa
[11]: https://python.langchain.com/docs/use_cases/question_answering/how_to/chat_vector_db
[12]: https://python.langchain.com/docs/modules/memory/types/buffer
