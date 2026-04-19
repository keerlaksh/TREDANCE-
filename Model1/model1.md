# Self-Pruning Neural Network (Learnable Sparse MLP)

## Overview

This project implements a **self-pruning neural network** where individual connections are not statically removed, but instead learned through **continuous gate parameters**. The model learns both:
- classification performance
- connection sparsity

using a joint optimization objective.

---

## Key Idea

Instead of fixed weights, each connection is controlled by a learnable gate:

\[
W_{effective} = W \cdot \sigma(G)
\]

where:
- \( W \) = weight matrix
- \( G \) = learnable gate scores
- \( \sigma \) = sigmoid function

This allows the model to softly decide which connections are important.

---

## Architecture

A fully connected MLP is used:

- Input: CIFAR-10 images (3072 features)
- Layers:
  - 3072 → 512
  - 512 → 256
  - 256 → 128
  - 128 → 10
- Each layer uses:
  - BatchNorm
  - ReLU
  - PrunableLinear layer with learnable gates

---

## Sparsity Objective

The model is trained using:

\[
Loss = CrossEntropy + \lambda \cdot SparsityLoss
\]

where:

\[
SparsityLoss = \text{mean}(\sigma(5 \cdot G))
\]

This encourages gate values to move toward zero, effectively pruning connections.

---

## Training Strategy

- Optimizer: Adam
- Learning rate: 1e-3
- Dataset: CIFAR-10
- λ values tested: [0.001, 0.005, 0.01]

---

## Results

| λ      | Accuracy | Sparsity |
|--------|----------|----------|
| 0.001  | 53.16%   | 92.28%   |
| 0.005  | 52.95%   | 92.32%   |
| 0.01   | 53.38%   | 92.32%   |

---

## Observations

### 1. High sparsity (~92%)
The model naturally converges to high sparsity due to:
- negative-biased initialization of gate scores
- sigmoid sharpening (×5 scaling)
- weak influence of sparsity loss compared to classification loss

---

### 2. Stable accuracy (~53%)
Accuracy remains stable across λ because:
- sparsity level is already saturated
- model is capacity-limited (MLP on CIFAR-10)
- pruning removes redundant but not critical features

---

### 3. Weak λ sensitivity
Changing λ does not significantly affect results because:
- sparsity behavior is dominated by initialization and activation scaling
- loss contribution from sparsity is small relative to classification loss

---

## Key Insight

This project demonstrates that:

> Sparsity in neural networks is not controlled only by loss functions, but strongly influenced by initialization, activation sharpness, and architecture design.

---

## Future Improvements

- Replace MLP with CNN backbone
- Use hard-concrete / L0 regularization
- Anneal sigmoid temperature during training
- Add explicit threshold-based pruning stage
- Normalize sparsity loss magnitude relative to CE loss

---

## Conclusion

This model provides a step toward interpretable and efficient neural networks by learning which connections are necessary instead of assuming full connectivity.
