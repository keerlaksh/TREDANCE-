# 🧠 CNN Pruning Pipeline on CIFAR-10

This project implements a **realistic pruning pipeline** using a Convolutional Neural Network (CNN) on the CIFAR-10 dataset.

The pipeline follows a standard industry workflow:

1. Train a dense model
2. Apply pruning
3. Fine-tune the pruned model
4. Evaluate performance

---

# 📌 Model Architecture

### 🔹 SimpleCNN

* 2 Convolutional layers
* 2 Fully Connected layers
* MaxPooling for downsampling

```
Input (32x32x3)
→ Conv(32) → ReLU → Pool
→ Conv(64) → ReLU → Pool
→ FC(256) → ReLU
→ FC(10)
```

---

# ⚙️ Training Details

* Dataset: CIFAR-10
* Optimizer: Adam
* Learning Rate: 0.001
* Loss: CrossEntropy
* Training Epochs: 5
* Fine-tuning Epochs: 3

---

# ✂️ Pruning Strategy

### 🔹 Magnitude-Based Pruning

* Removes weights with **lowest absolute values**
* Uses threshold based on quantile (70%)

```
threshold = quantile(|weights|, pruning_amount)
```

* Weights below threshold → set to **0**

---

# 📊 Results

## 🔹 Before Pruning (Training Phase)

| Epoch | Accuracy |
| ----- | -------- |
| 1     | 62.41%   |
| 2     | 68.22%   |
| 3     | 70.59%   |
| 4     | 71.93%   |
| 5     | 71.20%   |

---

## 🔹 After Pruning

* **Sparsity:** 70%
* **Pruned Weights:** 749,302 / 1,070,432

---

## 🔹 After Fine-Tuning

| Epoch | Accuracy |
| ----- | -------- |
| 1     | 72.12%   |
| 2     | 71.69%   |
| 3     | 71.83%   |

---

# 🔍 Key Observations

### ✅ High Accuracy Retention

* Even after removing **70% weights**, accuracy remains ~72%

---

### ✅ Fine-Tuning Recovers Performance

* Slight drop after pruning is recovered
* Model stabilizes quickly

---

### ✅ Efficient Compression

* Significant reduction in parameters
* Maintains strong performance

---

# 🏆 Final Performance

* **Final Accuracy:** ~71.8%
* **Sparsity:** 70%
* **Compression:** High
* **Stability:** Strong

---

# 📈 Conclusion

This model demonstrates that:

* Magnitude pruning is **simple yet powerful**
* CNNs are **robust to heavy pruning**
* Fine-tuning is **critical after pruning**
* This approach is **industry-standard and practical**

---

# 🚀 Future Improvements

* Structured pruning (filter/channel pruning)
* Pruning during training (dynamic pruning)
* Knowledge distillation
* Use deeper architectures (ResNet, VGG)
* Hardware-aware pruning

---

# 🧾 Summary

👉 Train → Prune → Fine-tune = Effective compression
👉 CNN + pruning = Best real-world performance
👉 70% sparsity with minimal accuracy loss is achievable

---
