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
- Use `get_state` to get state of the current checkpoint, use `get_state_history` to get the history of states
- Use `stream(None, {thread_id})` to execute the graph from the current state
- Use `stream(None, {checkpoint_id} {thread_id})` to **replay** (not execute) the graph from a specific checkpoint - we can also **execute** the graph from a checkpoint by updating the state with a new message and passing _id_ of an existing message. Doing so overrides the existing message and creates a new checkpoint and ultimately makes the graph execute.
- When using the SDK you can simply supply a _checkpoint_id_ to get the same behavior
- Use pydantic BaseModel to define state to enforce types
- Use input, output and overall state to filter state, i.e. when accepting and returning messages to the user (c.f. `StateGraph(OverallState, input=InputState, output=OutputState)`)
- Reduce the number of messages sent to a model during a long conversation in order to reduce latency and costs.
  - Use `trim_messages` to reduce the number of messages send to the model
  - Alternatively, create a summary of the conversation after n messages, trim all but the last two messages and continue

### Human-in-the-loop (HITL)

- HITL can be used for approval, e.g. before calling tools, debugging and editing graph state
- Breakpoints are one option to implement HITL. Another option is to use dummy nodes with breakpoints and call `update_state(as_node={node_name})` to update the state as if the node had been called
- We can pass breakpoints to a compiled graph via the API by using _interrupt_before_ or _interrupt_after_
- Use `update_state` (when using the client library) or `client.threads.update_state` (when using the API) to update the state of the graph when a breakpoint is hit
- Use `raise NodeInterrupt` to dynamically interrupt the execution of a graph

### Parallelization

- We can parallelize nodes by constructing the graph accordingly

### Sub graphs

- Top level and sub graphs communicate with each other via overlapping keys in the state
- In order to write to the same key, we need to define a reducer for the key, i.e. add, for both _input_ and _output_ keys
- Subgraphs makes traces much easier to readable

### Map Reduce

- Use `Send` to automatically generate nodes for specific tasks on the fly (map)
- In order reduce the results of the tasks, use a LLM to do the heavy lifting, i.e. choosing the best joke from a list of jokes and custom logic to select it, i.e. selecting the best joke by indexing into a list of jokes

### Tools

- Use `ToolNode` and `tools_condition` to easily call tools

### Streaming

- LangGraph supports synchronous (`stream`) and asynchronous (`astream`) streaming
- State can be streamed in _updates_ (updates only), _values_ (full state after each node has been called) and _messages_ mode
- When using the API to invoke graphs, state can be accessed via the _data_ attribute

## Deployment

- LangGraph requires the LangGraph API running on the LangGraph server to compile and execute graphs
- Server can be deployed locally, within the cloud or as SaaS via LangSmith
- Use SaaS solution to make graphs available
  - to business users via LangGraph Studio hosted in LangSmith
  - to applications by using the LangGraph SDK

## Misc

- We can identify messages by using the _name_ field in `AiMessage` and `HumanMessage`
- We can force a model to format its output by calling `with_structured_output(SCHEMA).invoke()` and providing a pydantic Schema
- Use `Image(graph.get_graph().draw_mermaid_png())` to get a visual representation of the graph in a notebook (import `Image` from `IPython.display)
- Use pydantic Fields to add descriptions to state schemas

## Resources

- LangGraph Studio
- LangGraph SDK
- Pydantic
