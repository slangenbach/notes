# Functions, Tools and Agents with LangChain

Notes on the [short course][1] from DeepLearning.AI

## Function Calling

### OpenAI

- OpenAI [function calling][2] does _not_ directly call a function, instead it tells us which of our function to call and which parameters to use
- We can control whether the model calls a function by passing `{"name": "your_function_name"}` or _none_ to the `function_call` parameter
- In order to actually call a function, pass a list of potential functions to the model and let the model decide to use a function. If it decides to do so, use _arguments_ output of the model to call your function _locally_ and pass the result back to the model

### LangChain

- Use Pydantic classes (using the _Field_ object) and the `convert_pydantic_to_openai_function()` util to create OpenAI function definitions from Pydantic classes
- Then use `.bind()` to pass the function definition to the model

## LangChain Expression Language (LCEL)

- [LCEL][3] supports async, batch and streaming out of the box. I t also supports fallbacks, parallelism and automated logging. Use it!
- [RunnableMap][4] allows us to pass inputs to the prompt
- Use `.bind()` to pass additional parameters at runtime
- Use `.with_fallbacks()` and pass a list of chains to try different chains when one throws an error
- Use `.batch()` and pass a list of dicts to run requests in parallel
- Use `.stream()` to stream completions
- All of the above methods can be called asynchronously by adding _a_ to the function name

## Tagging and Extraction

- We can let the model tag our inputs, i.e. determine its sentiment and language
- We can also let the model extract information from our inputs, i.e. a list of information about people
- Use `JsonOutputFunctionsParser()` to extract a Python dict from JSON object returned by the OpenAI completion request
- Use `JsonKeyOutputFunctionsParser()` to just extract a single key of the dict
- Use chain`.map()` to process lists of inputs within a chain

## Tools and Routing

- LangChain comes with [built-in tools][5]
- Use the `@tool` decorator to define your own tools. You can add additional metadata to the tool by defining a Pydantic model and pass it to the _args_schema_ parameter of the decorator
- Use `format_tool_to_openai_function` to convert LangChain tools to OpenAI functions format
- You can also get a OpenAI function definition from an Open**API** definition using `openapi_spec_to_openai_fn`
- Use the `OpenAIFunctionsAgentOutputParser` to easily parse tools and tool inputs selected by model. Then define a _route_ function checking the type of the agent response and either returning the output or invoking the chosen tool

## Conversational Agents

- Use the `AgentExecutor` class to run agents. Under the hood the agent runs a while loop, invoking a chain (potentially calling functions) until an object of class _AgentFinish_ is returned.
- In order to handle chat history and agent thoughts, add `MessagePlaceholder` classes to the `ChatPromptTemplate`

[1]: https://www.deeplearning.ai/short-courses/functions-tools-agents-langchain/
[2]: https://platform.openai.com/docs/guides/function-calling
[3]: https://python.langchain.com/docs/expression_language/
[4]: https://api.python.langchain.com/en/latest/schema/langchain.schema.runnable.base.Runnable.html#langchain.schema.runnable.base.Runnable
[5]: https://python.langchain.com/docs/integrations/toolkits/vectorstore
