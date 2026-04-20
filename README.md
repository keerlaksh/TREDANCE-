# 🧠 Self-Pruning Neural Networks on CIFAR-10

This project explores **different pruning strategies** on neural networks trained on CIFAR-10, focusing on the trade-off between **accuracy and sparsity**.

---

# 📌 Models Overview

We implemented and compared **4 different models**:

### 🔹 Model 1 — Fixed Self-Pruning MLP (Improved Gates)

* Uses **scaled sigmoid (5×)** for sharper pruning
* Negative gate initialization (encourages early pruning)
* Includes **BatchNorm + deeper architecture**
* Uses **normalized sparsity loss**

---

### 🔹 Model 2 — Self-Pruning with β Annealing (Advanced)

* Uses **annealed sigmoid (β increases over time)**
* Mimics **Hard-Concrete pruning behavior**
* Gradually forces gates → binary (0 or 1)
* More **controlled and aggressive pruning**

---

### 🔹 Model 3 — Basic Self-Pruning MLP (Baseline)

* Simple sigmoid gates
* No sharpening, no normalization
* Standard L1 sparsity penalty
* Acts as **baseline model**

---

### 🔹 Model 4 — CNN + Magnitude Pruning (Traditional)

* Standard CNN trained normally
* Post-training **weight magnitude pruning**
* Followed by **fine-tuning**
* Represents **real-world industry approach**

---

# 📊 Final Results Comparison

| Model                     | Type                    | Accuracy   | Sparsity  |
| ------------------------- | ----------------------- | ---------- | --------- |
| **Model 1**               | Improved MLP Pruning    | ~53.3%     | ~92.3%    |
| **Model 2**               | β-Annealed Pruning      | ~51.1%     | ~96.3%    |
| **Model 3 (best λ=1e-5)** | Basic MLP Pruning       | **56.24%** | **7.81%** |
| **Model 3 (λ=1e-4)**      | Balanced MLP            | 56.04%     | 36.69%    |
| **Model 3 (λ=1e-3)**      | Aggressive MLP          | 52.43%     | 57.77%    |
| **Model 4**               | CNN + Magnitude Pruning | **~71.8%** | **70%**   |

---

# 🔍 Key Observations

### 1. Accuracy vs Sparsity Trade-off

* Increasing pruning → **higher sparsity but lower accuracy**
* Classic ML trade-off clearly visible

---

### 2. Model-wise Insights

#### ✅ Model 1 (Fixed Pruning)

* Very **high sparsity (~92%)**
* Stable training
* Accuracy moderate (~53%)
* Good for **extreme compression**

---

#### ✅ Model 2 (β Annealing)

* **Highest sparsity (~96%)**
* Smooth transition from soft → hard pruning
* Slight drop in accuracy (~51%)
* Best for **research-level pruning techniques**

---

#### ⚠️ Model 3 (Baseline)

* Best accuracy at **low sparsity**
* Poor pruning effectiveness
* Gates remain mostly active unless λ is high
* Good **starting point, but not practical**

---

#### 🏆 Model 4 (CNN + Magnitude Pruning)

* **Best accuracy (~72%)**
* Strong pruning (70%) with minimal loss
* Most **realistic and production-ready**

---

# 🏆 Final Verdict

### 🔥 Best Overall Model: **Model 4 (CNN + Magnitude Pruning)**

✔ Highest accuracy
✔ Good sparsity
✔ Stable and industry-standard

---

### 🧪 Best Research Model: **Model 2 (β Annealing)**

✔ Most advanced pruning mechanism
✔ Achieves extreme sparsity (~96%)
✔ Demonstrates true self-pruning behavior

---

### ⚖️ Best Trade-off Model: **Model 3 (λ = 1e-4)**

✔ Balanced accuracy (~56%)
✔ Moderate sparsity (~36%)
✔ Good compromise setup

---

# 📈 Conclusion

* **MLP-based self-pruning models** struggle to maintain high accuracy on CIFAR-10.
* **Gate sharpening + normalization (Model 1 & 2)** significantly improves pruning effectiveness.
* However, **CNN + magnitude pruning still dominates** in practical scenarios.
* Advanced techniques like **β annealing** show promise for future research in **dynamic sparse networks**.

---

# 🚀 Future Improvements

* Replace MLP with **CNN + gating**
* Use **structured pruning (channels/filters)**
* Add **knowledge distillation**
* Try **Lottery Ticket Hypothesis**
* Implement **hard binary gates (straight-through estimator)**

---

# 🧾 Summary

👉 If you want **accuracy → use CNN pruning**
👉 If you want **extreme sparsity → use β annealing**
👉 If you want **learning/practice → use baseline MLP**

---
