# Practical Deep Learning for Coders

Notes for the 2022 edition of the course.

## Lesson 1

* "How do I get this data into my model?" is way more important than tweaking neural network architectures
* Datablocks -> DataLoaders -> Learner (check out the [Data block tutorial][1] and the high level data loader classes)
* Check Pytorch image models ([timm][2])
* For tabular data we usually can't fine tune models and thus use `fit_one_cycle()` instead of `fine_tune()`
* On a high level, neural networks work the following: Multiply inputs by weights -> put them through model -> calculate loss -> update weights

## Lesson 2

* Check out [AI Quizzes][3]
* Train your model _before_ you clean the data and use `ImageClassifierCleaner` to clean them 
* Use `RandomResizedCrop` to get different, cropped variations of an image
* Check out [Gradio][4] and [HuggingFace Spaces][5] (HF spaces can be used as a model endpoint as well)
* Check out [nbdev][6] and [notebook2script][7]
* Write your own UI using TypeScript and hook it up to HF spaces API!
* Bonus: Write an iOS App that does the same thing

## Lesson 3

* Check out [Paperspace][8]
* [timm][2] models can be used in fastai by specifying their name as a string in `vision_learner`
* Inspect the contents of a model by accessing `learner.model` - use `learner.get_submodule()` to get information on a sub module
* Get the categories of a multi-class prediction by accessing `learner.dls.vocab`
* Use `interact` from [ipywidgets][9] to get UI widgets to control input to a function
* Gradient dissent is just calculating the loss a function, calculating its gradients and decreasing them slightly
* ReLU returns 0 if linear function is <= 0 and the actual output of the function otherwise
* >ReLU means replace negatives with zeros
* >Deep Learning is using gradient dissent to set some parameters to make a wiggly function (which is just the addition of many RELUs - or something similar) to match your data
* Start your project with simple, small models and spend time on the data - trying better architectures is the _very last step_
* Check out [matrixmultiplication.xyz][10] for a visual walkthrough of matrix multiplication
* GPUs are great a matrix multiplication
* Read chapter 4 of Deep Learning for Coders with fastai & Pytorch book to get an even deeper understanding of the content in lesson 3

## Lesson 4

* >Transformers take good advantage of modern TPUs
* Check out [Python for Data Analysis 3rd Edition][11]
* [deberta-v3][12] is a good base model for NLP
* Try [ULMFiT][13] for long documents (>= 2000 words) instead of transformers
* Review [How and why to create a good validation set][14] and [The problem with metrics[...]][15] article
* Tokenization transforms words in documents into a numeric representation
* Always check you inputs (training data) and outputs (predictions)

## Lesson 5

### Neural Nets

* >Take the log of things which can grow exponentially, i.e. money
* >Add 1 to NAN values before taking the log
* A tensor is just a matrix
* Rank of a tensor referrs to its dimensions
* Use mean absolute value to get started with a loss function
* Methods with an underscore at the end, are executed _in-place_ in PyTorch
* Use the sigmoid function to keep prediction between 0 and 1 if dealing with a binary target
* >You need to know about what happens to the inputs in the first layer and what happens to the outputs in the last layer
* Use the @ operator to mulitply matrics in PyTorch
* You can slice a vector with _None_ to turn it into a matrix: vec[:, None]
* Dive into hand-crafted deep learning code in PyTorch!
* In 2023, tabular data still requires careful feature engineering and works well with tree-based models
* Check out fastai's improved learning rate finder and choose on between slide and valley
* Calling _test_dl on dataloaders will give you a pipeline containing all preprocessing steps from the original learner (which can then be used on the test set)

### Random Forests

* Use pandas categorical dtype when feeding categorical data into tree-based models
* >A binary split is something that turns the rows of your data into two groups
* >Always get a baseline with a "OneR" model (decision tree with a single binary split)

## Lesson 6

* >A way to think about Gini: How likely is it, if pulling an item from a sample, that you pick the same item twice in a row?
* Check out RandomForest feature importances to get a sense of the relevant features of a large data set
* Using 100 trees is a good rule of thumb
* Out-of-bag (OOB) error allows to check if a RandomForest is overfitting without using a separate validation set
* [Partial Dependence Plots][16] are great, use them!
* [Treeinterpreter][17] creates feature importance plots for a single prediction
* Check out [explained.ai][18]
* Check out [best vision models for fine-tuning][19] to select proper models for computer vision
* Check out [test time augementation (TTA)][20]

## Lesson 7

* Gradient accumulation is a technique to run models requiring lager batch sizes on small GPUs
* It works by calculating the loss for every item in the batch, but delaying the update of the weights (up to a certain threshold)
* Use the [DataBlock API][21] to create DataLoaders having 2 targets
* In order to create a multi-target model (that predicts two - or more - targets), create corresponding DataLoaders, adapt the error and loss functions (define the correct columns manually and create a combined loss functions adding up the results from the individual loss functions)
* Use cross-entropy-loss when predicting multipe targets
* Check out [Things that confused me about cross-entropy][22] article
* >All of the loss functions in PyTorch have two versions, they come as classes (including params to tune) and functions

### Collaborative Filtering

* >An embedding is just looking something up in an array
* Calculate latent factors (e.g. things that people like about movies):
    - start off with _random weights_ for latent factors and users
    - calculate the dot product between user preference and movies
    - calculate root mean squared error between actuals and predictions
    - optimize using SGD


## Lesson 8

[1]: https://docs.fast.ai/tutorial.datablock.html
[2]: https://timm.fast.ai/
[3]: https://aiquizzes.com/
[4]: https://www.gradio.app/
[5]: https://huggingface.co/docs/hub/spaces
[6]: https://nbdev1.fast.ai/
[7]: https://nbdev1.fast.ai/export.html#notebook2script
[8]: https://www.paperspace.com/
[9]: https://ipywidgets.readthedocs.io/en/latest/examples/Using%20Interact.html
[10]: http://matrixmultiplication.xyz/
[11]: https://wesmckinney.com/book/
[12]: https://huggingface.co/microsoft/deberta-v3-small
[13]: https://arxiv.org/abs/1801.06146v5
[14]: https://www.fast.ai/posts/2017-11-13-validation-sets.html
[15]: https://www.fast.ai/posts/2019-09-24-metrics.html
[16]: https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_partial_dependence_visualization_api.html#sphx-glr-auto-examples-miscellaneous-plot-partial-dependence-visualization-api-py
[17]: https://github.com/andosa/treeinterpreter
[18]: https://explained.ai/
[19]: https://www.kaggle.com/code/jhoward/the-best-vision-models-for-fine-tuning
[20]: https://docs.fast.ai/learner.html#tta
[21]: https://docs.fast.ai/data.block.html
[22]: https://chris-said.io/2020/12/26/two-things-that-confused-me-about-cross-entropy/