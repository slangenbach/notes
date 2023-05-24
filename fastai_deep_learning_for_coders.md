# Practical Deep Learning for Coders

Notes for the 2022 edition of the course.

## Lesson 1
* "How do I get this data into my model?" is way more important than tweaking neural network architectures
* Datablocks -> DataLoaders -> Learner (check out the [Data block tutorial][1] and the high level data loader classes)
* Check Pytorch image models ([timm][2])
* For tabular data we usually can't fine tune models and thus use `fit_one_cycle()` instead of `fine_tune()`
* On a high level, neural networks work the following: Multiply inputs by weights -> put them through model -> calculate loss -> update weights

[1]: https://docs.fast.ai/tutorial.datablock.html
[2]: https://timm.fast.ai/