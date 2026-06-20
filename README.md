# Hamming PUF ML Attack

## Overview
This repository contains the code and report for predicting the responses of a Hamming PUF using a linear model. Melbo's Hamming PUF design uses a TPM cryptoprocessor to store a 32-bit secret word `s`. It takes a 32-bit challenge `c`, calculates the bitwise XOR `m = c ⊕ s`, and finds the partial Hamming weights of all even locations (`he`) and odd locations (`ho`). If the product `he · ho` is greater than or equal to a secret threshold `t`, the response is 1; otherwise, it is 0. 

The goal of this project is to prove that these low-level operations can be broken by showing that a linear model can perfectly predict the responses given enough challenge-response pairs (CRPs).

## Repository Contents

* **`771ass1.ipynb`**: The core Python script containing two mandatory methods:
    * `my_map()`: Takes raw 32-bit challenges and maps each to a `D`-dimensional feature vector using a mathematical map `ϕ(c)`.
    * `my_params()`: Returns a Python dictionary of suitable hyperparameter values compatible with the `set_params()` method of the `sklearn.svm.LinearSVC` class.
* **CS771_Assignment1.pdf**: Contains detailed mathematical derivations and experimental outcomes, including:
    * The mathematical derivation of the explicit map `ϕ : {0, 1}^32 → R^D` and the corresponding linear model `w`, `b`.
    * Calculations showing the exact dimensionality `D` needed for the linear model.
    * Outcomes of experiments using `LinearSVC` and `LogisticRegression` on the public dataset, reporting how hyperparameters (`loss`, `C`, `tol`, `penalty`) affected training time and test accuracy.
    * A plot of train size vs test accuracy over at least 10 trials (ranging from 100 to 5000 train points) to find the minimum number of train CRPs needed for 95%, 97%, and 99% test accuracy.

## Data
The dataset provided for this task consists of:
* **Train Set**: 7500 CRPs
* **Test Set**: 2500 CRPs

Each row contains 33 bits: the first 32 numbers correspond to the challenge bits, and the final number corresponds to the 1-bit response.

## Usage
The code in `771ass1.ipynb` is designed to be compatible with Google Colab validation code. The custom feature vector map creates the new dimensionality required to learn the linear model and accurately predict the Hamming PUF responses without relying on PUF-specific constants like the secret word `s` or threshold `t` within the map itself.
