# Finetuning Large Language Models

Notes on the short course from [DeepLearning.AI][1]

## General

* Rule of thumb for data required to finetune a model: 1000 inputs and outputs

## Why should you finetune a model?

* By finetunig a model you can make it change its behavior and gain more knowledge
* Prompting is great for generic projects and prototypes, while finetunig is suitable for domain-specific use cases.
* Use finetuning to:
    - supply a model with more data than what would fit in an (engineered) prompt
    - ensure consistent output
    - reduce hallucinations
    - adapt it to a specific use case
    - host your custom LLM on-premise within your private cloud
    - reduce costs per request
    - provide guardrails


## Where does finetunig fit in the modeling process?

* Finetuning for generative tasks is not well defined: 
    - In contrast to finetuning a vision model, finetuning a generative model updates the weights of the entire model, not just a part of it
    - The training objective does not change (next token prediction)
* Finetuning usually happens after pretraining
* Finetuning tasks include _extraction_ (same text in, less text out, i.e. reading) and _expansion_ (same text in, more text out, i.e. writing)

##


[1]: https://www.deeplearning.ai/short-courses/finetuning-large-language-models/