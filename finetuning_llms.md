# Fine-tuning Large Language Models

Notes on the short course from [DeepLearning.AI][1]

## General

* Rule of thumb for data required to fine-tune a model: 1000 inputs and outputs
* _Hydrating_ a prompt means adding data to it
* _p3.2xlarge_ instances are able to host 7B parameter models for inference (1B parameters for trainings)
* Check out [llama][3] library from Lamini

## Why should you fine-tune a model?

* By fine-tuning a model you can make it change its behavior and gain more knowledge
* Prompting is great for generic projects and prototypes, while fine-tuning is suitable for domain-specific use cases.
* Use fine-tuning to:
    - supply a model with more data than what would fit in an (engineered) prompt
    - ensure consistent output
    - reduce hallucinations
    - adapt it to a specific use case
    - host your custom LLM on-premise within your private cloud
    - reduce costs per request
    - provide guardrails

## Where does finetunig fit in the modeling process?

* fine-tuning for generative tasks is not well defined: 
    - In contrast to fine-tuning a vision model, fine-tuning a generative model updates the weights of the entire model, not just a part of it
    - The training objective does not change (next token prediction)
* fine-tuning usually happens after pre-training
* fine-tuning tasks include _extraction_ (same text in, less text out, i.e. reading) and _expansion_ (same text in, more text out, i.e. writing)
* Expansion  tasks are usually much harder to accomplish than extraction tasks

## What is instruction-tuning?

* Instruction tuning teaches model to behave more like a chatbot
* Either make use of existing Q&A data (FAQs, support conversations, chat logs) or use an LLM (c.f. [Alpaca][2]) to generate/turn data into the correct format

## Data preparation

* Get high quality, diverse and real data as instruction-response pairs
    - Make sure to include pairs where the question is about something _not_ related to your issue, and the answers is something like: _Let's keep the discussion relevant to <your issue>_
* Concatenate pairs and insert into a prompt template
* Tokenize data applying padding (to make embeddings same length) and truncation (to respect maximum prompt length)
* Split into training and tests set
* Store result as JSON lines

## Training

* Use huggingface library and custom dataset to train (fine-tune) model
* Start with a small (400m - 1B parameters) model

## Evaluation

* Human evaluation is often most reliable option
* Alternative is to use LLM benchmarks, i.e. [ARC][4], [HellaSwag][5], [MMLU][6], [TruthfulQA][7], etc. (domain-specific models may however perform poorly on such general purpose tasks)
* Error analysis is helpful to understand flaws (misspellings, overly lengthy answers, repetitions) of base models before fine-tuning


[1]: https://www.deeplearning.ai/short-courses/finetuning-large-language-models/
[2]: https://crfm.stanford.edu/2023/03/13/alpaca.html
[3]: https://lamini-ai.github.io/python_library/
[4]: https://deepgram.com/learn/arc-llm-benchmark-guide
[5]: https://deepgram.com/learn/hellaswag-llm-benchmark-guide
[6]: https://deepgram.com/learn/mmlu-llm-benchmark-guide
[7]: https://openai.com/research/truthfulqa