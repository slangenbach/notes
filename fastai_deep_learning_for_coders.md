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
* >Deep Learning is using gradient dissent to set some parameters to make a wiggly function (which is just the addition of many RELUs - or something similar) to match your data
* Start your project with simple, small models and spend time on the data - trying better architectures is the _very last step_
* Check out [matrixmultiplication.xyz][10] for a visual walkthrough of matrix multiplication
* GPUs are great a matrix multiplication
* Read chapter 4 of Deep Learning for Coders with fastai & Pytorch book to get an even deeper understanding of the content in lesson 3


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