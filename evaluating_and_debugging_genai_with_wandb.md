# Evaluating and debugging generative AI

Notes on the short course from DeepLearning.AI[1]

## General

* WandB seems like a more intuitive, polished-up version of MLflow

## Training

* WandB allows [sampling (logging) of image][2] during training, which comes in handy when dealing with Computer Vision and Diffusion models
* Models produced during training runs can be shared with a team by putting them into the model registry

## Tracking

* Aside from logging usual metrics, WandB allows capturing outputs (via tables or tracer) for LLMs, i.e. system prompts, user prompts, responses, etc.
* WandB integrates with [LangChain][4] and [HuggingFace][5]

## Evaluation

* Use [tables][3] to log information (images, label, etc.) in a structured way
* Tables can be shared with teams within a report


[1]: https://www.deeplearning.ai/short-courses/evaluating-debugging-generative-ai/
[2]: https://docs.wandb.ai/ref/python/data-types/image
[3]: https://docs.wandb.ai/guides/tables/tables-walkthrough
[4]: https://docs.wandb.ai/guides/prompts/quickstart
[5]: https://docs.wandb.ai/tutorials/huggingface