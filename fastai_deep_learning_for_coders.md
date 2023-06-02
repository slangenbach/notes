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


[1]: https://docs.fast.ai/tutorial.datablock.html
[2]: https://timm.fast.ai/
[3]: https://aiquizzes.com/
[4]: https://www.gradio.app/
[5]: https://huggingface.co/docs/hub/spaces
[6]: https://nbdev1.fast.ai/
[7]: https://nbdev1.fast.ai/export.html#notebook2script
