---
title: Embedding Layer
parent: ML
has_children: false
---

## Embedding Layer in Keras
#### Embedding vs Dense
Embedding Layer in Keras is no different from the Dense Layer. However, the calculation is faster.
Let's say $X_1$ is the first input to train. Assume $X_1$ is one-hot-encoded.
Weight matrix will be initialized with random variables, but the dimension will be what we specify it to be.
For example, since $X_1$, the input matrix, has input dimension of 2x3, weight matrix has to have 3x"something"
dimension. If we want the "something" to be 5, which maps a 3 dimension bag of words one-hot-encoded matrix to that of a 5 dimension,
this would make the weights multiplied in the Embedding Layer to be of size 3x5. ($X'_1$ is the state before one-hot encoding.)

$$X'_1 = \begin{bmatrix}
2\\
3
\end{bmatrix}$$

$$X_1 = \begin{bmatrix}
0 & 1 & 0\\
0 & 0 & 1
\end{bmatrix}$$

$$W = \begin{bmatrix}
w_{11} & w_{12} & w_{13} & w_{14} & w_{15}\\
w_{21} & w_{22} & w_{23} & w_{24} & w_{25}\\
w_{31} & w_{32} & w_{33} & w_{34} & w_{35}\\
\end{bmatrix}$$

Then, we multiply $W$ and $X_1$ during feed forward phase.

$$
Y_1 = X_1W = \begin{bmatrix}
w_{21} & w_{22} & w_{23} & w_{24} & w_{25}\\
w_{31} & w_{32} & w_{33} & w_{34} & w_{35}\\
\end{bmatrix}
$$

We can simply see that $Y_1$ precisely holds exact same rows from $W$. Since one-hot-encoding basically captures a single
row of $W$, there is no need to multiply all the zeros. The difference between Embedding Layers and Dense Layers lie
in the fact that Embedding Layers neglect all the unneccessary zeros to be computed. In fact, Embedding Layers simply call the
index of rows in $W$ with numbers in $X'_1$.

[Source](https://stackoverflow.com/questions/47868265/what-is-the-difference-between-an-embedding-layer-and-a-dense-layer)

#### A Column of $W$ is a node.
From the $W$ above, the number of columns is the number of output matrices. So in this case, 5 matrices are generated from $WX$.
More precisely, a single hidden layer node's weight corresponds to a column of $W$. First column of $W$ is the first hidden node's
weights. Therefore, if there are 5 columns in $W$, it is natural to think that there are 5 nodes and 5 output matrices to be passed
to the next layer of DNN.

