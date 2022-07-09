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
w_{11} & w_{12} & w_{13} & w_{14} & w_{15}\\
w_{21} & w_{22} & w_{23} & w_{24} & w_{25}\\
w_{31} & w_{32} & w_{33} & w_{34} & w_{35}\\
\end{bmatrix}$$

Then, we multiply $W_1$ and $X_1$ during feed forward phase.

$$
Y_1 = X_1W_1 = \begin{bmatrix}
w_{21} & w_{22} & w_{23} & w_{24} & w_{25}\\
w_{31} & w_{32} & w_{33} & w_{34} & w_{35}\\
\end{bmatrix}
$$

We can simply see that $Y_1$precisely holds exact same rows from $W_1$. Since one-hot-encoding basically captures a single
row of $W_1$, there is no need to multiply all the zeros. The difference between Embedding Layers and Dense Layers lie
in the fact that Embedding Layers neglect all the unneccessary zeros to be computed.
