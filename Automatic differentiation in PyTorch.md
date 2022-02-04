# [Automatic differentiation in PyTorch](https://paperswithcode.com/paper/automatic-differentiation-in-pytorch)

## Key ideas
* Automatic differentiation module of PyTorch, a library designed to enable rapid research on ML models
* Builds upon LuaTorch, Chained, Autograd

## Background
* PyTorch supports reverse-mode automatic diferentiation of scalar functions
* Features:
  - Dynamic: defines the function to be differentiated by running the computation as opposed to specifying a static graph
  - Immediate: runs tensor computations ASAP as opposed to materializing a forward graph with lazy eval. Notice this allows for pipeline computations but gives up batching opportunities
  - In-place operations
  - No tape: Traditional reverse-mode differentiation records a tape to avoid topological sort. PyTorch returns the subset of the computation graph relevant to its computation.
  - Core logic in C++

## Interface
* Write code as operating tensor operations directly, however user actually manipulates variables
* Variables contain metadata for automatic differentiation
* Variables support backward() method which compute the gradient of all variables involved in the computation
* "variable.grad" accumulates the gradient - design inherited from Chained
* Extensions: user might write forward/backward functions to define custom differentiable operators
  - forward computes the operation
  - backward extends the vector-Jacobian product

## Implementation
* Variables are wrapper around a Tensor, with a reference to a graph of Function objects
* The graph is an immutable representation of derivatives of the computed function
* Variables are mutable pointers to this graph
* A function is a closure that has all context necessary to compute vector-Jacobian products
```
y = x.tanh()
y.add_(3)
y.backward()
```
* This instructs PyTorch to make an in-place operation on y - however this is unsound if y was saved to be used in the backwards computation
* Making a copy of y when saving it would be inefficient, so PyTorch will fail at runtime when differentiating this program
* Version-counter per variable is used to mitigate this issue
* Aliasing also affects original variables, e.g:
```
y = x[:2]
x.add_(3)
y.backward()
```
* Causes some elements of y to be updated. Hence, PyTorch rejects this program. In the future this restriction might be relaxed


