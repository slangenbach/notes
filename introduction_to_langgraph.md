# Introduction to LangGraph

## General

- Agent ~ control flow defined by LLM
- Agents can have varying amount of control (from router to fully autonomous)
- Balance reliability with control
- ReAct: act -> observe -> reason -> repeat
- Memory persists graph state across agent invocations

## Graphs

- Use state of node to reason about it
- Use reducer function, e.g. `add` from operator or LangGraph's `add_messages`, to add messages to list of messages.
  Alternatively, use `MessageState` class to define state uses `add_message` under the hood
- Use custom reducer to handle edge cases
- Pass ids to messages in order to overwrite or delete them
- Use `ToolNode` and `tools_condition` to easily call tools
- Use MemorySaver to persist graph state (not required when using LangGraph Studio)
- Use pydantic BaseModel to define state to enforce types

## Deployment

- LangGraph requires the LangGraph API running on the LangGraph server to compile and execute graphs
- Server can be deployed locally, within the cloud or as SasS via LangSmith
- Use SaaS solution to make graphs available
  - to business users via LangGraph Studio hosted in LangSmith
  - to applications by using the LangGraph SDK

## Resources

- LangGraph Studio
- LangGraph SDK
