# NN_Optimization
Experimenting with learning rates, layer activations, initializers, and optimizers

#HW3: Comparing Hyperparameters
* Note that much of the base code for this homework has been adopted from tensorboard tutorials found at: https://github.com/tensorflow/tensorboard/tree/master/docs/r2

#HW3: Comparing Hyperparameters
* Note that much of the base code for this homework has been adopted from tensorboard tutorials found at: https://github.com/tensorflow/tensorboard/tree/master/docs/r2

## Structure of homework

#### Each section starts with a general discussion and notes on the hyper parameter of interest
* - this discussion ends with a brief description of the choices of hyperparameter _flavors_ for initializers, optimizers, and activations

#### Each section is then tested seperately based on choices discussed above
* Note that the "control" choices for non-selected hparams are as follows:
* - Initializers: default/no argument
* - Optimizer: adam
* - Activation: relu

Based on personal experience, these are the best balance of performance vs computational efficiency, _for this dataset_

#### Finally, we run a grid search on all combinations of activation functions, initializers, and optimizers. 
* We allow for default learning rates in tihs section, to comprae each optimizers _base case_ performance. This is especially important given that some optimizers have variable learning rates

* Final discussion of results

### Activations
* softmax is the 'default' or gold standard
* - outputs probability function over all classes, weights sum to 1. Computationally efficient and "simple" to understand

* ELUs.. 'exponential linear unit'
* - elu: The exponential linear activation: x if x > 0 and alpha * (exp(x)-1) if x < 0.
* - selu: adds scale factor to elu Activations

* relu: what is difference when using relu _alone_ vs softmax?

* "Classic" activation functions:
* - tanh
* - sigmoid
* - hard_sigmoid
* - exponential
* - linear

* Note: _advanced_ activations exist
* - ex, leaky relu
* - more in depth to implement/not just callable functions

- Relu v Sigmoid v Softmax
- Relu is simpler than sigmoid, often better in practice (easier to compute)
- Softmax loss computation is efficient, better than ReLU

## Initializers
* Pass these inits as strings or callable (to customize arguments)
* Usually two main arguments, _within layer definition_
* - kernel init
* - bias init -- often just set to zero

### Built-in initializers
* Constants, e.g.
* - Zeros
* - Ones

* Random Norm
* - mean, stdev arguments, and seed
* - Also see _truncated normal_ ... values > 2 sigma are discarded and redrawn
* - Note that TruncatedNormal is the recommended initializer

* Random Uniform
* - inits gradient with a uniform dist
* - set min/max values, and seed for reproducibility

* Variance scaling: truncated norm or uniform w/adaptation of variance to prior or subsequent layer shape
* - Lecun_uniform: draws samples from uniform, with empirical limit based on fan_in size (size of prior layer)
* - also see Lecun norm

* Orthogonal: generates random orthogonal matrix..
* - find "exact" solutions.. but at what cost?

* Identity matrix: 2D only.. pads with zeros if not square

* Glorot_normal: truncatd normal with heuristic for std_dev
* - based on average of fan_in, fan_out shapes (# units)
* - aka Xavier normal

* Glorot_uniform: similar approach to Glorot_normal, but for uniform

* He_normal, He_Uniform.. similar to Glorot norm/uniform

### Choices for experiment
* Random Uniform - "base case", gradients initialized with uniform dist
* TruncatedNorm - utilizes random norm, with truncated values > +/- 2 sigma, redrawn
* Glorot_normal - expands on truncated norm, but contains heuristic based on prior and subsequent layers

## Optimizers
* Methods and details of weight updates, per layer
* Will almost always accompany a loss function.. _how_ to leverage the loss function
* Helps mitigate vanishing gradient issues, and optimize learning rate(s)

* clipping
* - clipnorm and clipvalue provide a knob to prevent exploding weights

### Common optimizers
* SGD: supports concept of momentum, decaying learning rate
* RMSProp: in a nut shell, divide gradient by running average of recent magnitude
* - can tune learning rate
* Adagrad: "adaptive subgradient methods" .. not recommended to tune
* - Adadelta: uses a window of past gradients to adapt learning rates, rather than _all_ past gradients
* Adam: Popular optimizer -- "essentially RMSProp with momentum"
* - read paper.. details on stochastic optimization using this alg
* Adamax: variant of Adam using different norm (infiniti)
* Nadam: Adam, but with Nesterov momentum
* - how does Nesterov momentum differ?

* Research importance of weight initialization for these methods.. Nadam, ex

### Choices for experiment
* SGD
* RMSProp - provides means to normalize gradient based on running average of recent ||gradient|| value
* Adam - similar to RMSProp, with _momentum_ concept

