# Introduction to LangGraph

## General

- Agent ~ control flow defined by LLM
- Agents can have varying amount of control (from router to fully autonomous)
- Balance reliability with control
- ReAct: act -> observe -> reason -> repeat
- Memory persists graph state across agent invocations

## Graphs

### State

- Use state of node to reason about it
- Use reducer function, e.g. `add` from operator or LangGraph's `add_messages`, to add messages to list of messages.
  Alternatively, use `MessageState` class to define state - it uses `add_message` under the hood
- To add a message, i.e. a system message, to the state, simply wrap it in a list and add it with `state["messages"]`
- Use custom reducers to handle edge cases
- Pass ids to messages in order to overwrite or delete them
- Use `MemorySaver` to persist graph state (not required when using LangGraph Studio)
  - Is state automatically persisted when graphs are deployed in LangGraph Cloud?
- Use thread ids to group state graphs persisted via `MemorySaver` checkpoints
- Use pydantic BaseModel to define state to enforce types
- Use input, output and overall state to filter state, i.e. when accepting and returning messages to the user (c.f. `StateGraph(OverallState, input=InputState, output=OutputState)`)
- Reduce the number of messages sent to a model during a long conversation in order to reduce latency and costs.
  - Use `trim_messages` to reduce the number of messages send to the model
  - Alternatively, create a summary of the conversation after n messages, trim all but the last two messages and continue

### Tools

- Use `ToolNode` and `tools_condition` to easily call tools

### Streaming

- LangGraph supports synchronous (`.stream`) and asynchronous (`astream`) streaming
- State can be streamed in _updates_ (updates only), _values_ (full state after each node has been called) and _messages_ mode

## Deployment

- LangGraph requires the LangGraph API running on the LangGraph server to compile and execute graphs
- Server can be deployed locally, within the cloud or as SasS via LangSmith
- Use SaaS solution to make graphs available
  - to business users via LangGraph Studio hosted in LangSmith
  - to applications by using the LangGraph SDK

## Misc

- You can identify messages by using the _name_ field in `AiMessage` and `HumanMessage`

## Resources

- LangGraph Studio
- LangGraph SDK
