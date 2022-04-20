---
tags:
  - tensorflow
  - python
  - ml
  - machine-learning
  - data-processing
  - modelling
---

# TensorFlow
## [Install](https://www.tensorflow.org/install)
Pip Install
  - `pip install tensorflow`

[Anaconda Install](https://docs.anaconda.com/anaconda/user-guide/tasks/tensorflow/)
  - `anaconda create -n tf tensorflow`
  - `conda install tensorflow`

> [!NOTE]+
> Anaconda tensorflow package is typically out-of-date from pip/venv packages.

### [GPU Support](https://www.tensorflow.org/install)

#### Hardware
The following GPU-enabled devices are supported:
  * NVIDIA® GPU card with CUDA® architectures 3.5, 5.0, 6.0, 7.0, 7.5, 8.0 and higher than 8.0. See the list of [CUDA®-enabled GPU cards](https://developer.nvidia.com/cuda-gpus).
  * For GPUs with unsupported CUDA® architectures, or to avoid JIT compilation from PTX, or to use different versions of the NVIDIA® libraries, see the [Linux build from source](https://www.tensorflow.org/install/source) guide.
  * Packages do not contain PTX code except for the latest supported CUDA® architecture; therefore, TensorFlow fails to load on older GPUs when `CUDA_FORCE_PTX_JIT=1` is set. (See [Application Compatibility](http://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#application-compatibility) for details.)

> [!ATTENTION]- NVIDIA CUDA GPU is Required

#### Software
The following NVIDIA® software must be installed on your system:
  - [NVIDIA® GPU drivers](https://www.nvidia.com/drivers) —CUDA® 11.2 requires 450.80.02 or higher.
  - [CUDA® Toolkit](https://developer.nvidia.com/cuda-toolkit-archive) —TensorFlow supports CUDA® 11.2 (TensorFlow >= 2.5.0)
  - [CUPTI](http://docs.nvidia.com/cuda/cupti/) ships with the CUDA® Toolkit.
  - [cuDNN SDK 8.1.0](https://developer.nvidia.com/cudnn) [cuDNN versions](https://developer.nvidia.com/rdp/cudnn-archive)).
  - _(Optional)_ [TensorRT 7](https://docs.nvidia.com/deeplearning/tensorrt/archives/index.html#trt_7) to improve latency and throughput for inference on some models.


## Basics
> [!INFO]- References
> [[References#^reftensorflowcheatsheet]]

### Layers

| Layers                 | Code                                                         | Usage                                                        |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Dense                  | `tf.keras.layers.Dense(units, activation, input_shape)`      | Dense layer is the regular deeply connected neural network layer. It is most common and frequently used layer. |
| Flatten                | `tf.keras.layers.Flatten()`                                  | Flattens the input.                                          |
| Conv2D                 | `tf.keras.layers.Conv2D(filters, kernel_size, activation, input_shape)` | Convolution layer for two-di­men­sional data such as images. |
| MaxPooling2D           | `tf.keras.layers.MaxPool2D(pool_size)`                       | Max pooling for two-di­men­sional data.                      |
| Dropout                | `tf.keras.layers.Dropout(rate)`                              | The Dropout layer randomly sets input units to 0 with a frequency of `rate` at each step during training time, which helps prevent overfitting. |
| Embedding              | `tf.keras.layers.Embedding(input_dim, output_dim, input_length)` | The Embedding layer is initialized with random weights and will learn an embedding for all of the words in the dataset. |
| GlobalAveragePooling1D | `tf.keras.layers.GlobalAveragePooling1D()`                   | Global average pooling operation for temporal data.          |
| Bidirectional LSTM     | `tf.keras.layers.Bidirectional(tf.keras.layers.LSTM(units, return_sequence))` | Bidirectional Long Short-Term Memory layer                   |
| Conv1D                 | `tf.keras.layers.Conv1D(filters, kernel_size, activation, input_shape)` | Convolution layer for one-dimentional data such as word embeddings. |
| Bidirectional GRU      | `tf.keras.layers.Bidirectional(tf.keras.layers.GRU(units))`  | Bidirectional Gated Recurrent Unit                           |
| Simple RNN             | `tf.keras.layers.SimpleRNN(units, activation, return sequences, input_shape)` | Fully-connected RNN where the output is to be fed back to input. |
| Lambda                 | `tf.keras.layers.Lambda(function)`                           | Wraps arbitrary expressions as a `Layer` object.             |

### Models

| Code                                                         | Usage                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `model = tf.ker­as.S­eq­uen­tia­l(l­ayers)`                  | Sequential groups a linear stack of layers into a tf.ker­as.M­odel. |
| `model.co­mpi­le(­opt­imizer, loss, metrics)`                | Configures the model for training.                           |
| `history = model.fit(x, y, epoch)`                           | Trains the model for a fixed number of epochs (itera­tions on a dataset). |
| `history = model.fit_generator(train_generator, steps_per_epoch, epochs, validation_data, validation_steps)` | Fits the model on data yielded batch-­by-­batch by a Python generator. |
| `model.ev­alu­ate(x, y)`                                     | Returns the loss value & metrics values for the model in test mode. |
| `model.pr­edi­ct(x)`                                         | Generates output predic­tions for the input samples.         |
| `model.su­mma­ry()`                                          | Prints a string summary of the network.                      |
| `model.save(path)`                                           | Saves a model as a TensorFlow SavedModel or HDF5 file.       |
| `model.stop_training`                                        | Stops training when true.                                    |
| `model.save('path/my_model.h5')`                             | Save a model in HDF5 format.                                 |
| `new_model = tf.keras.models.load_model('path/my_model.h5')` | Reload a fresh Keras model from the saved model.             |

### Activation Functions

| Name    | Usage                                     |
| ------- | ----------------------------------------- |
| relu    | the default activation for hidden layers. |
| sigmoid | binary classi­fic­ation.                  |
| tanh    | faster conver­gence than sigmoid.         |
| softmax | multiclass classi­fic­ation.              |

### Optimizers

| Name     | Usage                                                        |
| -------- | ------------------------------------------------------------ |
| Adam     | Adam combines the good properties of Adadelta and RMSprop and hence tend to do better for most of the problems. |
| SGD      | Stochastic gradient descent is very basic and works well for shallow networks. |
| AdaGrad  | Adagrad can be useful for sparse data such as tf-idf.        |
| AdaDelta | Extension of AdaGrad which tends to remove the decaying learning Rate problem of it. |
| RMSprop  | Very similar to AdaDelta.                                    |

### Loss Functions

| Name                          | Usage                                                        |
| ----------------------------- | ------------------------------------------------------------ |
| MeanSquaredError              | Default loss function for regression problems.               |
| MeanSquaredLogarithmicError   | For regression problems with large spread.                   |
| MeanAbsoluteError             | More robust to outliers.                                     |
| BinaryCrossEntropy            | Default loss function to use for binary classi­fic­ation problems. |
| Hinge                         | It is intended for use with binary classi­fic­ation where the target values are in the set {-1, 1}. |
| SquaredHinge                  | If using a hinge loss does result in better perfor­mance on a given binary classi­fic­ation problem, is likely that a squared hinge loss may be approp­riate. |
| CategoricalCrossEntropy       | Default loss function to use for multi-­class classi­fic­ation problems. |
| SparseCategoricalCrossEntropy | Sparse cross-­entropy addresses the one hot encoding frustr­ation by performing the same cross-­entropy calcul­ation of error, without requiring that the target variable be one hot encoded prior to training. |
| KLD                           | KL divergence loss function is more commonly used when using models that learn to approx­imate a more complex function than simply multi-­class classi­fic­ation, such as in the case of an autoen­coder used for learning a dense feature repres­ent­ation under a model that must recons­truct the original input. |
| Huber                         | Less sensitive to outliers                                   |

### Hyperparameters

| Parameter            | Tips                                                         |
| -------------------- | ------------------------------------------------------------ |
| Hidden Neurons       | The size of the output layer, and 2/3 the size of the input layer, plus the size of the output layer. |
| Learning Rate        | [0.1, 0.01, 0.001, 0.0001]                                   |
| Momentum             | [0.5, 0.9, 0.99]                                             |
| Batch Size           | Small values give a learning process that converges quickly at the cost of noise in the training process. Large values give a learning process that converges slowly with accurate estimates of the error gradient. The typical sizes are [32, 64, 128, 256, 512] |
| Conv2D Filters       | Earlier 2D convolutional layers, closer to the input, learn less filters, while later convolutional layers, closer to the output, learn more filters. The number of filters you select should depend on the complexity of your dataset and the depth of your neural network. A common setting to start with is [32, 64, 128] for three layers, and if there are more layers, increasing to [256, 512, 1024], etc. |
| Kernel Size          | (3, 3)                                                       |
| Pool Size            | (2, 2)                                                       |
| Steps per Epoch      | sample_size // batch_size                                    |
| Epoch                | Use callbacks                                                |
| Embedding Dimensions | vocab_size ** 0.25                                           |
| Truncating           | `post`                                                       |
| OOV Token            | `<OOV>`                                                      |

### Preprocessing

**ImageDataGenerator**

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# Image augmentation
train_datagen = ImageDataGenerator(
      rescale=1./255,
      rotation_range=40,
      width_shift_range=0.2,
      height_shift_range=0.2,
      shear_range=0.2,
      zoom_range=0.2,
      horizontal_flip=True,
      fill_mode='nearest')
validation_datagen = ImageDataGenerator(rescale=1/255)

# Flow training images in batches of 128 using train_datagen generator
train_generator = train_datagen.flow_from_directory(
        '/tmp/horse-or-human/',  # This is the source directory for training images
        target_size=(300, 300),  # All images will be resized to 300x300
        batch_size=128,
        # Since we use binary_crossentropy loss, we need binary labels
        class_mode='binary')

# Flow training images in batches of 128 using train_datagen generator
validation_generator = validation_datagen.flow_from_directory(
        '/tmp/validation-horse-or-human/',  # This is the source directory for training images
        target_size=(300, 300),  # All images will be resized to 300x300
        batch_size=32,
        # Since we use binary_crossentropy loss, we need binary labels
        class_mode='binary')
```

**Tokenizer, Text-to-sequence & Padding**

```python
import tensorflow as tf
from tensorflow import keras


from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences

sentences = [
    'I love my dog',
    'I love my cat',
    'You love my dog!',
    'Do you think my dog is amazing?'
]

tokenizer = Tokenizer(num_words = 100, oov_token="<OOV>")

# Key value pair (word: token)
tokenizer.fit_on_texts(sentences)
word_index = tokenizer.word_index

# Lists of tokenized sentences
sequences = tokenizer.texts_to_sequences(sentences)

# Padded tokenized sentences
padded = pad_sequences(sequences, maxlen=5)

print("\nWord Index = " , word_index)
print("\nSequences = " , sequences)
print("\nPadded Sequences:")
print(padded)

```

**One-hot Encoding**

```python
ys = tf.keras.utils.to_categorical(labels, num_classes=3)
```

### Metrics

**F1-Score**

```python
import keras.backend as K

def f1_score(y_true, y_pred):
    true_positives = K.sum(K.round(K.clip(y_true * y_pred, 0, 1)))
    possible_positives = K.sum(K.round(K.clip(y_true, 0, 1)))
    predicted_positives = K.sum(K.round(K.clip(y_pred, 0, 1)))
    precision = true_positives / (predicted_positives + K.epsilon())
    recall = true_positives / (possible_positives + K.epsilon())
    f1_val = 2*(precision*recall)/(precision+recall+K.epsilon())
    return f1_val
```

### Visualizations

**Accuracy and Loss**

```python
import matplotlib.pyplot as plt
#-----------------------------------------------------------
# Retrieve a list of list results on training and test data
# sets for each training epoch
#-----------------------------------------------------------
acc      = history.history['accuracy']
val_acc  = history.history['val_accuracy']
loss     = history.history['loss']
val_loss = history.history['val_loss']

epochs   = range(len(acc)) # Get number of epochs

#------------------------------------------------
# Plot training and validation accuracy per epoch
#------------------------------------------------
plt.plot  ( epochs, acc )
plt.plot  ( epochs, val_acc )
plt.title ('Training and validation accuracy')
plt.figure()

#------------------------------------------------
# Plot training and validation loss per epoch
#------------------------------------------------
plt.plot  ( epochs, loss )
plt.plot  ( epochs, val_loss )
plt.title ('Training and validation loss')
```

**Intermediate Representations**

```python
import numpy as np
import random
from   tensorflow.keras.preprocessing.image import img_to_array, load_img

# Let's define a new Model that will take an image as input, and will output
# intermediate representations for all layers in the previous model after
# the first.
successive_outputs = [layer.output for layer in model.layers[1:]]

#visualization_model = Model(img_input, successive_outputs)
visualization_model = tf.keras.models.Model(inputs = model.input, outputs = successive_outputs)

# Let's prepare a random input image of a cat or dog from the training set.
cat_img_files = [os.path.join(train_cats_dir, f) for f in train_cat_fnames]
dog_img_files = [os.path.join(train_dogs_dir, f) for f in train_dog_fnames]

img_path = random.choice(cat_img_files + dog_img_files)
img = load_img(img_path, target_size=(150, 150))  # this is a PIL image

x   = img_to_array(img)                           # Numpy array with shape (150, 150, 3)
x   = x.reshape((1,) + x.shape)                   # Numpy array with shape (1, 150, 150, 3)

# Rescale by 1/255
x /= 255.0

# Let's run our image through our network, thus obtaining all
# intermediate representations for this image.
successive_feature_maps = visualization_model.predict(x)

# These are the names of the layers, so can have them as part of our plot
layer_names = [layer.name for layer in model.layers]

# -----------------------------------------------------------------------
# Now let's display our representations
# -----------------------------------------------------------------------
for layer_name, feature_map in zip(layer_names, successive_feature_maps):
  
  if len(feature_map.shape) == 4:
    
    #-------------------------------------------
    # Just do this for the conv / maxpool layers, not the fully-connected layers
    #-------------------------------------------
    n_features = feature_map.shape[-1]  # number of features in the feature map
    size       = feature_map.shape[ 1]  # feature map shape (1, size, size, n_features)
    
    # We will tile our images in this matrix
    display_grid = np.zeros((size, size * n_features))
    
    #-------------------------------------------------
    # Postprocess the feature to be visually palatable
    #-------------------------------------------------
    for i in range(n_features):
      x  = feature_map[0, :, :, i]
      x -= x.mean()
      x /= x.std ()
      x *=  64
      x += 128
      x  = np.clip(x, 0, 255).astype('uint8')
      display_grid[:, i * size : (i + 1) * size] = x # Tile each filter into a horizontal grid

    #-----------------
    # Display the grid
    #-----------------

    scale = 20. / n_features
    plt.figure( figsize=(scale * n_features, scale) )
    plt.title ( layer_name )
    plt.grid  ( False )
    plt.imshow( display_grid, aspect='auto', cmap='viridis' ) 
```

### Callbacks

**Learning Rate Scheduler**

```python
model = tf.keras.models.Sequential([
    tf.keras.layers.Dense(10, input_shape=[window_size], activation="relu"), 
    tf.keras.layers.Dense(10, activation="relu"), 
    tf.keras.layers.Dense(1)
])

lr_schedule = tf.keras.callbacks.LearningRateScheduler(
    lambda epoch: 1e-8 * 10**(epoch / 20))
optimizer = tf.keras.optimizers.SGD(lr=1e-8, momentum=0.9)
model.compile(loss="mse", optimizer=optimizer)
history = model.fit(dataset, epochs=100, callbacks=[lr_schedule], verbose=0)
```

**End of Training Cycles**

```python
import tensorflow as tf

class myCallback(tf.keras.callbacks.Callback):
  def on_epoch_end(self, epoch, logs={}):
    if(logs.get('accuracy')>0.6):
      print("\nReached 60% accuracy so cancelling training!")
      self.model.stop_training = True

mnist = tf.keras.datasets.fashion_mnist

(x_train, y_train),(x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

callbacks = myCallback()

model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(28, 28)),
  tf.keras.layers.Dense(512, activation=tf.nn.relu),
  tf.keras.layers.Dense(10, activation=tf.nn.softmax)
])
model.compile(optimizer=tf.optimizers.Adam(),
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(x_train, y_train, epochs=10, callbacks=[callbacks])
```

### Transfer Learning

```python
import os

from tensorflow.keras import layers
from tensorflow.keras import Model
from tensorflow.keras.applications.inception_v3 import InceptionV3

local_weights_file = '/tmp/inception_v3_weights_tf_dim_ordering_tf_kernels_notop.h5'

pre_trained_model = InceptionV3(input_shape = (150, 150, 3), 
                                include_top = False, 
                                weights = None)

pre_trained_model.load_weights(local_weights_file)

for layer in pre_trained_model.layers:
  layer.trainable = False
  
# pre_trained_model.summary()

last_layer = pre_trained_model.get_layer('mixed7')
print('last layer output shape: ', last_layer.output_shape)
last_output = last_layer.output

from tensorflow.keras.optimizers import RMSprop

# Flatten the output layer to 1 dimension
x = layers.Flatten()(last_output)
# Add a fully connected layer with 1,024 hidden units and ReLU activation
x = layers.Dense(1024, activation='relu')(x)
# Add a dropout rate of 0.2
x = layers.Dropout(0.2)(x)                  
# Add a final sigmoid layer for classification
x = layers.Dense  (1, activation='sigmoid')(x)           

model = Model( pre_trained_model.input, x) 

model.compile(optimizer = RMSprop(lr=0.0001), 
              loss = 'binary_crossentropy', 
              metrics = ['accuracy'])
```

### Overfitting
* **[Augmentation](#preprocessing)** 
* **Reduce Model Complexity**
  * Reduce overfitting by training the network on more examples.
  * Reduce overfitting by changing the complexity of the network (network sturcture and network parameters).
* **Regularization**
* **Dropout Layer**

### TensorFlow Data Services
TensorFlow Datasets is a collection of datasets ready to use, with TensorFlow or other Python ML frameworks, such as Jax. All datasets are exposed as [`tf.data.Datasets`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset), enabling easy-to-use and high-performance input pipelines. To get started see the [guide](https://www.tensorflow.org/datasets/overview) and our [list of datasets](https://www.tensorflow.org/datasets/catalog).
