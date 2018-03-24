# Assignment 2

## 1. Tensorflow Softmax (25 points)
### (a)
See `q1_softmax.py`.

### (b)
See `q1_softmax.py`.

### (c)
In TensorFlow computations, the role of `placeholder` variables is to be fed with data on execution. Other graph operations will treat placeholder variables as their inputs. You should use feed dictionaries to feed data to placeholders. To add a comment, you should consider saving data directly as a `variable` in a graph if data is fit into your primary memory, since this will reduce I/O bottleneck problem significantly.

For implentations, see `q1_classifier.py`.

### (d)
See `q1.classifier.py`

### (e)
For implementations, see `q1_classifier.py`

In TensorFlow, when you call `sess.run(train_op)`, TensorFlow's automatic differentiation will calculate gradients for your computational graph and also update your variables with caluclated gradients. If you're curious about how TensorFlow handles autodifferentiation, See [Appendix of This Book](https://www.amazon.com/Hands-Machine-Learning-Scikit-Learn-TensorFlow/dp/1491962291) or [StackOverflow](https://stackoverflow.com/questions/36370129/does-tensorflow-use-automatic-or-symbolic-gradients).

```python
optimizer = tf.train.GradientDescentOptimizer(self.config.lr)
train_op = optimizer.minimize(loss)

(...)

with tf.Session() as sess:
    (...)
    _ = sess.run(train_op)
```

## 2. Neural Transition-Based Dependency Parsing
### (a)
|stack                         |buffer                                |new dependency     |transition           |
|------------------------------|--------------------------------------|-------------------|---------------------|
|[ROOT]                        |[I, parsed, this, sentence, correctly]|                   |Initial Configuration|
|[ROOT, I]                     |[parsed, this, sentence, correctly]   |                   |SHIFT                |
|[ROOT, I, parsed]             |[this, sentence, correctly]           |                   |SHIFT                |
|[ROOT, parsed]                |[this, sentence, correctly]           |parsed -> I        |LEFT-ARC             |
|[ROOT, parsed, this]          |[sentence, correctly]                 |                   |SHIFT                |
|[ROOT, parsed, this, sentence]|[correctly]                           |                   |SHIFT                |
|[ROOT, parsed, sentence]      |[correctly]                           |sentence -> this   |LEFT-ARC             |
|[ROOT, parsed]                |[correctly]                           |parsed <- sentence |RIGHT-ARC            |
|[ROOT, parsed, correctly]     |[]                                    |                   |SHIFT                |
|[ROOT, parsed]                |[]                                    |parsed <- correctly|RIGHT-ARC            |
|[ROOT]                        |[]                                    |ROOT -> parsed     |LEFT-ARC             |

### (b)
Since every word is shifted from buffer to stack and removed from the stack, two time steps are needed to handle one word. Thus, **A sentence containing n words will be parsed in 2\*n steps**. For example, above example in (a) was parsed in 10(=2\*5) steps.

### (c)
See `q2_parser_transitions.py`.

To add a comment, you should not write a code as below in `__init__()`.
```python
# Don't do this!
self.buffer = sentence
```

This will cause sentence to be modified in your code! Instead, You should use `copy` module or list comprehension. If you want to know more about this, See python books or websites which covers the referential structure of python `list` class or `deepcopy`. 

### (d)
See `q2_parser_transitions.py`.

### (e)
See `q2_initialization.py`.

### (f)
Considering the fact that expectation of **d**  is 1-p,

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{150}&space;\gamma&space;=&space;\dfrac{1}{1-p_{prop}}" target="_blank"><img src="https://latex.codecogs.com/png.latex?\dpi{150}&space;\gamma&space;=&space;\dfrac{1}{1-p_{prop}}" title="\gamma = \dfrac{1}{1-p_{prop}}" /></a>

### (g)
1. If beta_1 = 0.99, m will be almost same as the value before update, which means that updates do not vary as much. This will help learning by preventing parameters from bouncing near optimum. Also, by taking moving average of minibatch gradients, this might make gradient closer to true gradient over all dataset.

2. Model parameters which have small gradients will get larger updates. The Main idea is that we should scale updates similarly across mini-batches by adjusting learning rates of weights based on the magnitudes of their gradients. This will help learning by making parameters with small gradients escape from plateaus.

### (h)
See `q2_parser_model.py`.