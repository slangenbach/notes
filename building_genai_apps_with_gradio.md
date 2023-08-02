# Building Generative AI apps with Gradio

Notes on the short course from [DeepLearning.AI][1]

## General

* [Gradio][2] makes it easy to build user interfaces for AI apps
* Start with standard [interface][3] and use specific components, i.e. [TextBox][4] or [HighlightedText][5] for specific configuration
* Move to [Blocks][6] and [Block lLyouts][7] to build more sophisticated interfaces
* Use HuggingFace Inference Endpoints to use open source LLMs, i.e. [Llama2][8] or [StableBeluga][9] for chatbot apps
* Use the [ChatInterface][10] or [ChatBot][11] component to get started with chatbot apps - Note that it may be more convenient to use LangChain all chatbot functionality except from UI


[1]: https://www.deeplearning.ai/short-courses/building-generative-ai-applications-with-gradio/
[2]: https://www.gradio.app/docs/interface
[3]: https://www.gradio.app/docs/interface
[4]: https://www.gradio.app/docs/textbox
[5]: https://www.gradio.app/docs/highlightedtext
[6]: https://www.gradio.app/docs/blocks
[7]: https://www.gradio.app/docs/block-layouts
[8]: https://huggingface.co/meta-llama/Llama-2-7b-chat-hf
[9]: https://huggingface.co/stabilityai/StableBeluga-7B
[10]: https://www.gradio.app/docs/chatinterface
[11]: https://www.gradio.app/docs/chatbot