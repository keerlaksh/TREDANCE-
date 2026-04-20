# Self-Pruning Neural Network on CIFAR-10

## Overview
This project implements a self-pruning Multi-Layer Perceptron (MLP) on CIFAR-10 using learnable gates and L1 sparsity regularization. The model learns which weights are important and gradually removes unnecessary ones during training.

The goal is to study the trade-off between accuracy and sparsity by varying the sparsity coefficient (λ).

---

## Core Idea

Each weight in the network is controlled by a learnable gate:

Effective Weight = Weight × Gate

Gate is computed as:

Gate = sigmoid(gate_score)

- gate_score is a trainable parameter
- Gate values lie between 0 and 1
- Small gate values effectively remove weights

---

## Loss Function

Total Loss = Classification Loss + λ × Sparsity Loss

1. Classification Loss:
   - CrossEntropyLoss
   - Ensures the model learns the task

2. Sparsity Loss:
   - Sum of all gate values
   - Encourages gates to become small
   - Drives pruning

---

## Architecture

Input: 32 × 32 × 3 (flattened to 3072)

Layer 1: 3072 → 512 (ReLU)  
Layer 2: 512 → 256 (ReLU)  
Layer 3: 256 → 10 (Output)

All layers use prunable linear transformations.

---

## Experiment Setup

Three values of λ were tested:

- λ = 1e-5 (weak sparsity)
- λ = 1e-4 (moderate sparsity)
- λ = 1e-3 (strong sparsity)

Each model was trained for 10 epochs using Adam optimizer.

---

## Results

| Lambda  | Accuracy | Sparsity |
|--------|----------|----------|
| 1e-5   | 56.24%   | 7.81%    |
| 1e-4   | 56.04%   | 36.69%   |
| 1e-3   | 52.43%   | 57.77%   |

---

## Observations

1. Increasing λ increases sparsity:
   - Higher λ forces more gates toward zero
   - More weights are pruned

2. Accuracy vs Sparsity trade-off:
   - Low λ → high accuracy, low sparsity
   - High λ → high sparsity, lower accuracy

3. Best balance:
   - λ = 1e-4 gives good accuracy (~56%) with moderate sparsity (~37%)

---

## How Pruning Happens

During training:

- Useful weights:
  - Help reduce classification loss
  - Their gates remain high (close to 1)

- Useless weights:
  - Do not contribute to accuracy
  - Sparsity loss pushes their gates toward 0

---

## Sparsity Measurement

A weight is considered pruned if:

Gate < 0.01

Sparsity (%) = (Number of pruned weights / Total weights) × 100

---

## Gate Distribution

After training, gate values are visualized using a histogram.

- Values near 0 → pruned weights
- Values near 1 → important weights

This shows how the network separates useful and useless connections.

---

## Key Takeaways

- The model can automatically prune itself during training
- No manual pruning step is required
- λ controls the strength of pruning
- There is a clear trade-off between accuracy and sparsity

---

## Limitations

- MLP is not ideal for image data (CNNs perform better)
- Sparsity is unstructured (individual weights, not neurons)
- Very high λ can degrade performance

---

## Possible Improvements

- Use CNN instead of MLP
- Apply structured pruning (remove neurons/channels)
- Introduce annealing for better pruning control
- Fine-tune the model after pruning

---

## Conclusion

This experiment demonstrates that neural networks can learn both:
- What to learn (important weights)
- What to forget (pruned weights)

By adjusting λ, we can control the balance between model accuracy and efficiency.
