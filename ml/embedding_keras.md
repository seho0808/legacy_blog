---
title: Embedding Layer
parent: ML
has_children: false
---

## Embedding Layer in Keras
Embedding Layer in Keras is no different from the Dense Layer. However, the calculation is faster.
Let's say $X_1$ is the first input to train. Assume $X_1$ is one-hot-encoded.
Weight matrix will be initialized with random variables, but the dimension will be what we specify it to be.
For example, since $X_1$, the input matrix, has input dimension of 2x3, weight matrix has to have 3x"something"
dimension. We specify the "something" part by `model.add(tf.keras.layers.Embedding(3, something, input_length=10))`
If we want the something to be 5, which maps a 3 dimension bag of words one-hot-encoded matrix to that of a 5 dimension,
this would make the weights multiplied in the Embedding Layer to be of size 3x5.

$$X_1 = \begin{bmatrix}
0 & 1 & 0\\
0 & 0 & 1
\end{bmatrix}$$

$$W_1 = \begin{bmatrix}
w_11 & w_12 & w_13 & w_14 & w_15\\
w_21 & w_22 & w_23 & w_24 & w_25\\
w_31 & w_32 & w_33 & w_34 & w_35\\
\end{bmatrix}$$

Then, we multiply $W_1$ and $X_1$ during feed forward phase.

$$
X_1W_1
$$
