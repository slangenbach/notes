# HuggingFace NLP course 🤗

Notes on the course from [HuggingFace][1]

## General

* Transfer learning (TL) leverages knowledge of model A on task 1 for model B on task 2 (by initializing model B with the weights of model A)
* >TL drops the head of the pre-trained model, but keeps its body
* In NLP, predicting the next word and guessing random masked word are common pre-training tasks

## Misc

* Chose eu-west3 (Paris) or eu-north-1 (Stockholm) for the lowest CO2 emissions
* Use well-researched random search instead of grid search for hyperparameter tuning

## Transformers

### Architecture

#### Encoders

* Encoder transforms text into numerical representation (embeddings)
* Embeddings take into account context of words (via self-attention)
* Encoders are bi-directional (can access context on left _and_ right), auto-regressive (can produce new output taking into account previous input until stop sequence is reached) and good at extracting meaningful information (natural language understanding)
* Encoders are used for sequence classification (i.e. sentiment analysis), question answering, masked language modeling
* Examples: bert, roberta

#### Decoders

* Decoders are unidirectional (can only access context to their right _OR_ left)
* Decoders are great a generating sequence (natural language generation)
* Examples: GPT-2

#### Encoder-Decoders

* Decoder uses output for encoder as input
* Encoder understands entire input sequence and helps decoder to generate new words
* Further output of decoder is produced by taking previous output into account, as well as embeddings generated by encoder
* Encoders-Decoders are used for sequence-to-sequence modeling
* Examples: BART
* You can load an encoder and decoder model into an Encoder-Decoders model

### Tokenizers

* There exist 3 forms of tokenizers: word-based, character-based and subword-based tokenizers
* Word-based tokenizers split text into words based on separators (spaces, punctuation)
    - Each word gets a unique id
    - Very similar words will be assigned different ids and the model will learn different embeddings for them (bad)
    - Vocab of word-based tokenizers gets huge fast
    - Many out-of-vocab tokens
* Character-based tokenization splits text into characters
    - Smaller vocab than word-based approach, yet fewer out of vocab words
    - Approach might conflict with context size of language models (due to long sequences of characters provided as input)
* Subword-based tokenization splits text into subwords
    - Frequently used words are not split
    - Rare words are decomposed into meaningful subwords
    - Most SOTA models focused on English language use subword-based algorithm

### Padding

* Static padding adds padding tokens to all elements of the datasets. Doing so, ensures all
batches will have the same shape, yet a lot of batches will contain columns with _useless_ pad tokens
* Dynamic padding adds padding tokens to all elements in a batch. All elements in a batch will be of the 
same shape, yet batches will be of different shapes which might not work well on all accelerators (TPUs)
* Dynamic padding will usually be faster on CPUs _and_ GPUs (not TPUs)

### Package

* Pipeline is most high-level function available and can be used for tasks such as
    - sentiment-analysis
    - zero-shot-classification (providing your own labels)
    - text-generation
    - text completion (mask filling)
    - named-entity recognition (ner)
    - question-answering
    - translation
* Pipeline can be used with any model suited to the task you want to perform
* Pipeline consists of 3 stages: tokenize, model, postprocess
* AutoTokenizer loads correct tokenizer for model
* AutoTokenizer calls `tokenize` and `convert_tokens_to_ids` functions
* AutoTokenizer uses padding to stitch together embedding vectors of different sizes. It uses an attention mask (tensor of 0 and 1) to indicate which tokens should be taken into account by self-attention and which should be ignored
* Dynamic padding can be applied by passing the tokenizer to `DataCollatorWithPadding` class and using it
as the `collate_fn` parameters within the PyTorch `DataLoader`
* AutoModel class only loads model without pre-training head (which can't be used for tasks directly) (use AutoModelFor<TASK> class instead)
* During postprocessing logits (values of the last layer of a network before SoftMax is applied) are transformed into probabilities by calling SoftMax on them (use `model.config.id2label` to get them)
* Use `np.argmax` to turn probabilities into class predictions
* `Trainer` class enables training and fine-tuning of models
* A training loop consists of model -> compute loss -> compute gradients -> use optimizer to update weights
* Use `get_scheduler` function to do learning rate scheduling in PyTorch
* Use the [accelerator][2] package to optimize training for different infrastructure:
    - (Multiple) CPU, 
    - (Multiple) GPU (one one or more machines)
    - TPUs


## Datasets

* Load datasets from the hub via `load_dataset()` - you can also load custom datasets this way (from local disk or URL)
* Access train/valid/test datasets (and rows) by slicing the datasets object, i.e. `dataset["train"][:5]`
* Use the `features` method to get info about columns
* Use `map` method to apply preprocessing functions to all splits in the dataset (use `batched=True` to speed up)
* Use `selected` method to apply transformation for part of index only
* Use `load_metric` function to get evaluation metrics of dataset and use it within custom evaluation function
* datasets provides various function to manipulate datasets, i.e. `shuffle` (incl. train/test split), `select`, `filter`, `rename_column`, `remove_columns`, `flatten` (to handle nested columns) and `map`
* Use `dataset.set_format("pandas")` to interact with datasets as if it were pandas dataframes - there also exists `to_pandas` to cast to pandas datafames and `reset_format` to cast back
* Use `save_to_disk` to save data in arrow format (conserves splits and metadata, useful for re-training) or `to_parquet` to save in parquet format (need to save and load each split individually)
* datasets provides a streaming API to process _bigger-thank-disk_ datasets

## Hub

* Check out the [Hugging Face Hub Python library][3], especially for working in notebooks, managing repos and doing inference

## Tokenizers

* Tokenizer must be suitable to your training corpus (consider differences in language, style, characters, domain)
* Use `train_new_from_iterator` in order to adapt an existing tokenizer to your training corpus
* Fast tokenizers are backed by rust and take advantage of parallel processing of batches of text
* Token classification: Use offset mapping to get the span of text for each label
* Question answering: Model outputs two tensors (start- and end logits), containing the question and answer
* Use `tokenizer.backend_tokenizer.normalizer.normalize_str` to check how a tokenizer normalizes an input string
* _Pre-tokenization_ is applied after _normalization_ and applies rules to realize an initial split of text - use `pre_tokenize_str` to apply it
* Common tokenization algorithms include (**TODO**: go back to videos for details):
    - Byte Pair Encoding (BPE)
    - Wordpiece tokenization
    - Unigram tokenization
* You can build your own tokenizer using the tokenizers library and the relevant normalizers, pre_tokenizers, models (trainers), post_processors and decoders


 

[1]: https://huggingface.co/learn/nlp-course/chapter1/1
[2]: https://github.com/huggingface/accelerate
[3]: https://huggingface.co/docs/huggingface_hub/quick-start