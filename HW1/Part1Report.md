# Assignment 1, Part 1

PML&DL course
D.romanov, BS4-DS1
repo: https://github.com/einepeople/S20_PMLDL

# How to run?

Requirements: `pandas`,`sklearn`, `torch`
Put it into your local Jupyter environment or Google Colab and push "Run all" button.

# Brief task description

Create a neural network optimizer based on Simpulated Annealing algorithm.
Use it to optimize NN weights for an Iris classification task.

# Implementation

Simulated Annealing optimizer was implemented using corresponding PyTorch API -`Optimizer` base class was used for inheritance. 
Resulting `SimAnnealing` optimizer is not task-specific and may be used for other optimization problems as any other optimizer in the package.

Iris classification is arguably the simplest classification problem in the field. Because of that neural network was designed to be as easy as possible:

1. Linear 4-to-3 layer (3x4 weights matrix and 1x3 bias vector)
2. SELU activation (often ReLU is taken as a baseline, but due to very small network size it was decided to use more advanced one)
3. Linear 3-to-3 layer (3x3 weights matrix and 1x3 bias vector)
4. Softmax activation

Total of 27 neural network weights were initilized using Xavier Glorot's initilization method.
Dataset was scaled to *N(0,1)* using Sklearn StandardScaler and then splitted into train and test part. Test split ratio = 0.5.

# Experiment setup

SA optimizer was compared with Adadelta(AD) omptimizer from PyTorch package. Maximal test set accuracy was chosen to be a primary metric for every experiment. Experiment reports would contain accuracy score for both optimizers and iteration after which the model stopping creteria activated.

SA network stopping criterias were either value of T falling below 0.003, test accuracy achieving ay least 0.9, or a zero division error during acceptance ratio computation that indicates training loss rounding up to zero.

Adadelta stopping criterias were either test accuracy achieving ay least 0.9 or iteration count surpassing MAX_ITER number.

If hyperparameter value is not described in the experiment, it is equal to the default one. For hyperparameters change that does not affect AD, only SA result will be shown.
Default values of hyperparameters are:
* T = 2.0
* Anneal every (AE) = 5 iterations
* Annealing rate(ANR) = 0.99
* Variance of normal distribution for candidate solution sampling (P_VAR) = 0.05
* MAX_ITER = 2000



Some of conducted experiments and their results:

1. SEED = 20201, ANR = 0.999
    SA = 0.84 after 27400 iterations
    AD = 0.9067 after 178 iterations

2. SEED = 590, ANR = 0.999
    SA = 0.8533 after 26950 iterations
    AD = 0.9067 after 183 iterations

3. SEED = 590, ANR = 0.99
    SA = 0.9067 after 2590 iterations

4. SEED = 103, T = 10.0
    SA = 0.5733 after 3450 iterations
    AD = 0.9067 after 99 iterations

5. SEED = 103, AE = 1
    SA = 0.6267 after 500 iterations
    
6. SEED = 1999, P_VAR = 0.1
    SA = 0.7867 after 2550 iterations
    AD = 0.9067 after 111 iterations

7. SEED = 1999, P_VAR = 0.01
    SA = 0.7067 after 2550 iterations
    
8. SEED = 12345678
    SA = 0.8533 after 2800 iterations
    AD = 0.9067 after 136 iterations
    
# Best hyperparameters values

After more than 50 experiments optimal SA hyperparameters values were found to be:
* T = 2.0
* ANR = 0.99
* AE = 5
* P_VAR = 0.05

# Discussion

Adadelta optimizer definitely shown better results in both convergence speed and accuracy, but it used a very important prior (at the moment of optimization step) knowledge of gradient direction and magnitude.
SA shown comparable efficiency in accuracy and far worse time performance due to its stochastic nature. When lack of crucial prior knowledge is taken into account, the methodlooks promising, especially in cses, where gradient is unavailable.
In ordinary machine learning tasks I would definitely stick with the gradient-based optimizers, but for non-differentiable functions solutions like Simulated annealing, Metropolis-Hastings, Gibbs Sampling or Markov-chain Monte-Carlo(MCMC) family in general may be the only option.
Other research idea is to analyze MCMC-bsed optimizers behavior in higher dimensional spaces - optimization of a 1000 weights vector is far more complicated task, than the presented one with just 27 of them.
