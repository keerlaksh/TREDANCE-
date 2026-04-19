
# Self-Pruning Neural Network (CIFAR-10)

## Overview
This project implements a self-pruning neural network using learnable gates. The model automatically learns which weights are important and removes unnecessary ones during training.

Instead of pruning after training, pruning happens dynamically while training.

---

## Core Concept

Each weight is multiplied by a learnable gate:

Effective Weight = Weight × Gate

Gate is defined as:

Gate = sigmoid(β × gate_score)

- gate_score → trainable parameter
- β (beta) → controls how sharp pruning is
- Gate values range between 0 and 1

---

## Interpretation of Gates

- Gate ≈ 1 → weight is important (kept)
- Gate ≈ 0 → weight is unimportant (pruned)

---

## Loss Function

Total Loss = Classification Loss + λ × Sparsity Loss

1. Classification Loss:
   - Cross entropy loss
   - Ensures model learns the task

2. Sparsity Loss:
   - Mean of all gate values
   - Encourages gates to become small (pushes weights to zero)

---

## Beta Annealing (Very Important)

β increases gradually during training.

- Early training:
  - β small → smooth gates → learning phase

- Later training:
  - β large → sharp gates → pruning phase

Without this:
- Either no pruning happens
- Or pruning happens too aggressively

---

## Training Phases

1. Learning Phase (Early epochs)
   - Model learns useful features
   - Sparsity is low

2. Pruning Phase (Middle epochs)
   - Unimportant weights shrink
   - Sparsity increases

3. Stabilization Phase (Final epochs)
   - Model stabilizes
   - Accuracy plateaus
   - High sparsity achieved

---

## Architecture

Input (3072)
→ Fully Connected (512) + ReLU
→ Fully Connected (256) + ReLU
→ Fully Connected (10)

All layers are prunable.

---

## How Pruning Happens

During training:

- Useful weights:
  - Reduce classification loss
  - Their gate values increase
  - They survive

- Useless weights:
  - Do not help accuracy
  - Sparsity loss pushes gates to zero
  - They get pruned

---

## Observed Results

- Test Accuracy: ~50–55%
- Sparsity: ~96%

This means only ~4% of weights are actively used.

---

## Issues Observed

- Very high sparsity early in training (e.g., 87% at epoch 1)
- Caused by:
  - Too negative gate initialization
  - β starting too high

---

## Best Practices

1. Gate Initialization:
   Use mild negative values (e.g., -1 instead of -2)

2. Beta Schedule:
   Start small and increase gradually

3. Delayed Sparsity:
   Apply sparsity loss only after a few epochs

Example:
if epoch < 5:
    loss = classification_loss
else:
    loss = classification_loss + λ × sparsity_loss

---

## Limitations

- Lower accuracy compared to CNNs
- MLP does not capture spatial features well
- Excessive pruning can reduce performance

---

## Possible Improvements

- Use CNN instead of MLP
- Structured pruning (remove neurons instead of weights)
- Hard Concrete gates for binary pruning
- Fine-tuning after pruning

---

## Key Takeaway

The model learns:
- What is important
- What can be removed

This results in an efficient, compressed neural network without manual pruning.
