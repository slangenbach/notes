# Deeplearning for coders (fast.ai)

## Lesson 1

* Infinite flexible function: Universal approximation theorem
* All purpose parameter fitting: Stochastic Gradient Descent (SGD)
* Fast and scalable execution: GPUs
* Finding a good learning rate is very important (check out paper mentioned in class and `lr_find()` function in fastai module)
* Equally important is a reasonable value for epochs (iterations through the dataset with multiple iterations of gradient descent)
* Use data augmentation on increase the amount of data in your training and testing set
* Remember easy steps to create a world class identifier 

## Lessons 2

* >The best way to avoid overfitting is to get more data - use data augmentation to do so
    - Make sure to set `precompute=False` in order for data augmentation to have an effect
* A smart way to set a learning rate is to use stochastic gradient descent with restarts (SGDR) which uses cosine annealing to find an optimal learning rate
    - Make sure to use 3 cycles and set `cycle_len=1` and `cycle_mut=2` parameters in the `fit()` function to use SGDR (with optimal defaults)
* If you want to retrain precomputed layers of a network use differential learning rates, i.e. different learning rates per layer.
    - Make sure to `unfreeze()` layers before attempting to retrain them
    - Start with very small learning rates for early layers and increase gradually 
* Start learning on a specific image size, e.g. 244 and then continue with an increased size, e.g. 299, to get even better results
    - Use `set_data()` method to increase size of input images
* A good default architecture is resnet34. Use resnext50 as a second step

## Lesson 3

* If using architectures trained on imagenet (e.g. resnet50) to recognize similar objects in picture, freeze batch normalizations when unfreezing layers by using the method `bn._freeze(True)`
* Revisit the conv-excel-example whenever you to need to explain convolution to somebody
* RELU is short for rectified linear unit which literally means throwing away the negatives
* MaxPooling is essentially reducing the resolution of a layer
* >Tensor is just a fancy word for an array with more dimensions
* In deep learning an activation function is a non-linearity, e.g. RELU, Softmax, Sigmoid, etc.
* Softmax is basically dividing a value of category you want to predict (calculated by taking another value to the power of e, the base of the natural logarithm) by the sum of values for all categories
* Softmax generates positive values between 0 and 1
* >Remember softmax wants to pick a thing, it is not suited to multi-label classification
* Sigmoid is particularly suited for multi-label classification, because as opposed to softmax, multiple classes can have _high_ values
* Sigmoid asymptotes the top to 1 and the bottom 0
* Always train the fully connected layers to make them better than random and then unfreeze the other layers
* Use `get_cv()` function from fastai library to create validation sets
* Gradually learn on and increase image sizes (64 to 128 to 224) to avoid overfitting

## Lesson 4

* Dropout solves the problem of generalization/overfitting, because it randomly deletes activations - according to the parameter ps (which accepts an array for different dropout layers)
* Always monitor (validation-)loss because it is the only metric we can observe for the training, validation and test set
* When dealing with structured datasets you have to separate categorical (distance has no meaning) from continuous variables (distance has meaning)
* The amount of levels in a categorical variable is called cardinality
* >Always scale your data if you use neural nets
* >Make sure you have a strong understanding of your evaluation metric
* An embedding is basically a distributed, high-dimensional representation (of a categorical variable, e.g. weekday?)
* Rule of thumb for creating embedding matrices sizes: either 50 columns or (cardinality + 1)/2
* Another rule of thumb is convert variables into categorical variables as long as the cardinality is not too high

### NLP

* Language Model means predicting the next word in a sentence
* We can use a language model as a pre-trained network for sentiment analysis
* Tokenization is basically generating (word) fragments from a sentence/paragraph - it helps computers work with language more effectively as they can process text is smaller units.
* Vocab in NLP-jargon means the unique dictionary of words in a text

## Advice

* Spend time on code in notebooks instead on theory
* Before reading a paper, try to find a blog post about it
* Use SHIFT+TAB (several times; or use ? and ??) to see the arguments of a function in Jupyter
* Press H to see the keyboard shortcuts of Jupyter
* Data needs to be splitted into classes (cats, dogs) and splitted into train and validation
* Probabilities for predictions are on log-scale and need to be transformed via np.exp()
* Review scientific notation , e.g. 10^-1 = 0.1, 10^-2 = 0.01
* Set batch size to 64 and half it when you run into memory errors
* If you import `fastai.conv_learner`, everything else you need will be imported, too
* Use `%time` in conjunction with fit(), TTA() and predict() methods
* Use `FileLink()` with a valid path to get an URL to download stuff from Jupyter server
* The only math to really understand deep learning is logarithms and exponents (revisit street fighting mathematics)
* Use fastai data object to access train, validation and test datasets (`data.test_ds`) and loaders (using `next(iter(data.test_dl))`)
* Use `learn.summary()` got get an overview of the entire model
* Save huge structured datasets in feather/parquet format
* Just typing the learner object (usually _learn_) will print information about all its layers
* Use `proc_df()` and `ColumnarModelData.from_data_frame()` to easily work with structured data
* Use `add_datepart()` to easily get information on a date
* Use torchtext (pytorch's NLP library) for text analysis

## Trivia

* _Whole game_ teaching approach
