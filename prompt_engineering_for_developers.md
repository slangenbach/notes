# Prompt Engineering for Developers

Notes on the short course from [DeepLearning.AI][1]

## General

* Base LLM predicts next word based on text training data
* Instruction-tuned LLM uses reinforcement learning with human feedback (RLHF) to follow instructions
* Be aware of model hallucinations

## Guidelines

### Prompting principles

#### Write clear and specific instructions

* Use delimiters (backticks, quotes, tags, etc.) to clearly indicate distinct parts of the input
* Ask for structured output (HTML, JSON, YAML, etc.)
* Ask model to check whether conditions are satisfied
* Provide examples of the output you expect (few-shot prompting)

#### Give model time to think

* Specify steps required to complete a task
* Ask for output in specified format (not necessarily data format)
* Instruct model to work out its own solution and compare it with an example solution

## Iteration

* Note to self: How to refer to previous prompt using OpenAI Python package?

## Summarizing

* If summaries include topics not in focus, try using _extract_ instead of _summarize_
* Use loops to generate summaries from multiple inputs

## Inferring

* Just ask for the sentiment directly (and potentially limit the output to _positive_ or _negative_)
* Ask for emotions directly (make sure to ask for structured output as well)
* You can also ask for doing several tasks sequentially by specifying them in your prompt
* Ask for specifying topics discussed in a text directly

## Transforming

* Translate text from one language into another (or more)
* You can also infer the language of a prompt and then translate it into your desired language
* You can also translate into formal and informal variations of a language
* Check text for spelling and grammatical error by asking the model to _proofread_ and _re-write_ it
* Convert from one data structure to another, i.e. from Python dict to HTML

## Expanding

* Auto-generate responses to text (emails, tickets, etc.) by instructing the model to assume a persona (service assistant) and outline the steps it should take.
* Use `temperature` parameter to control randomness of model




[1]: https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/