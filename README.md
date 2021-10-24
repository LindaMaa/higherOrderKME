# Higher Order Kernel Mean Embeddings for Stochastic Processes

This library provides estimators for the first and second order Maximum Mean Discrepancies (MMDs) with GPU support.

## Installation

`pip install git+https://github.com/maudl3116/higherOrderKME.git` 

## Structure of the repository

- The `higherOrderKME` folder contains the implementation of the higher order MMDs (sigkernel.py) and higher order distribution regression algorithms (KES.py)
- The `examples` folder contains notebooks to perform two-sample tests, higher order distribution regression and causal graph discovery for stochastic processes.  
- The `n_vcn_utils` folder contains various utilies for the multibody interaction experiment (adapted from https://github.com/pairlab/v-cdn)
- The `option_utils` folder contains various utilies for the optimal stopping time experiment (adapted from https://github.com/HeKrRuTe/OptStopRandNN)

## How to use the library

```python
import torch
import higherOrderKME 
from higherOrderKME import sigkernel

# Specify the static kernel (for linear kernel use sigkernel.LinearKernel())
static_kernel = sigkernel.RBFKernel(sigma=0.5)

# Specify dyadic order for PDE solver (int > 0, default 0, the higher the more accurate but slower)
dyadic_order = 2

# Specify the hyperparameter for the estimation of the conditional KME
lambda_ = 1e-5

# Initialize the corresponding signature kernel
signature_kernel = sigkernel.SigKernel(static_kernel, dyadic_order)

# Synthetic data
batch, len_x, len_y, dim = 5, 10, 20, 2
x = torch.rand((batch,len_x,dim), dtype=torch.float64, device='cuda') # shape (batch,len_x,dim)
y = torch.rand((batch,len_y,dim), dtype=torch.float64, device='cuda') # shape (batch,len_y,dim)

# Compute the (classical) first order MMD distance between samples x ~ P and samples y ~ Q, where P,Q are two distributions on path space
mmd_order1 = signature_kernel.compute_mmd(x,y,order=1)

# Compute the second order MMD distance between samples x ~ P and samples y ~ Q, where P,Q are two distributions on path space
mmd_order2 = signature_kernel.compute_mmd(x,y,lambda_=lambda_,order=2)
```
