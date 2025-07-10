# 📄 Paper Summary: EfficientNet

**Title**: EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks  
**Link**: [https://arxiv.org/abs/1905.11946](https://arxiv.org/abs/1905.11946)  
**Authors**: Mingxing Tan, Quoc V. Le  
**Published by**: Google AI (ICML 2019)  

---

## ✅ Day 0 – Why I Chose This Paper

### 📌 Background & Motivation

After reading **MobileNetV2**, I was curious about what came next in the evolution of efficient CNNs.  
MobileNetV2 introduced **inverted residual blocks** and showed how to design lightweight architectures for mobile and edge devices.

While searching for the next step in the family tree, I discovered **EfficientNet**, which:

- Builds directly on MobileNetV2’s design principles  
- Proposes a novel **compound scaling** strategy to scale depth, width, and resolution **together**  
- Uses **Neural Architecture Search (NAS)** to discover a strong baseline model (EfficientNet-B0)  
- Achieves **SOTA accuracy** with significantly fewer FLOPs and parameters

This made it the **perfect follow-up paper** to dive into.

---

## ✅ Day 1 – Abstract, Introduction & Motivation

### 📌 Abstract Summary

EfficientNet introduces a new family of CNN models that balance accuracy and efficiency by proposing a **compound scaling method**.  
Instead of scaling network depth, width, or resolution arbitrarily, it scales all three dimensions **uniformly** based on a fixed set of coefficients.  
The base model EfficientNet-B0 is discovered using **Neural Architecture Search**, and larger variants (B1–B7) are derived via compound scaling.

EfficientNet models achieve **state-of-the-art accuracy** with **significantly fewer parameters and FLOPs**, making them ideal for both cloud and edge deployment.

---

### 📌 Introduction Insights

- Prior CNN models improved accuracy by simply **making networks deeper or wider**, which leads to inefficient computation.  
- EfficientNet solves this by introducing a **principled method** to scale all model dimensions together.  
- Compound scaling maintains a balance across depth, width, and resolution, leading to better accuracy and efficiency.  
- EfficientNet achieves top-1 ImageNet accuracy of **84.4%** with **8.4× fewer FLOPs** and **6.1× fewer parameters** than previous models like GPipe and ResNet.

---

### 📌 Problem Statement

Most existing scaling methods focus on only **one** dimension:

- Increase **depth** → may suffer from vanishing gradients  
- Increase **width** → higher memory usage  
- Increase **resolution** → higher computation cost  

There is no systematic rule for scaling them **together**.  
EfficientNet introduces a **compound coefficient φ** and a method to uniformly scale all three using constants (α, β, γ).

---

### 📌 Core Design Principles

1. **Search for a good baseline (EfficientNet-B0)** using Neural Architecture Search  
2. **Scale it up** with fixed scaling constants:  

$$
\text{depth} \propto \alpha^\phi,\quad 
\text{width} \propto \beta^\phi,\quad 
\text{resolution} \propto \gamma^\phi
$$

3. Maintain resource constraint:

$$
\alpha \cdot \beta^2 \cdot \gamma^2 \approx 2 \quad \text{(to double FLOPs per } \phi \text{)}
$$

This leads to a family of EfficientNet-B1 to B7.

---

### 📌 3-Line Summary

- EfficientNet proposes a **compound scaling method** that balances depth, width, and resolution.  
- It starts from a **NAS-searched baseline (B0)** and scales uniformly for larger variants.  
- The result is a set of models that are both **accurate and lightweight**, outperforming deeper/wider networks with fewer resources.

---

### 📌 Unfamiliar Terms

- **Compound scaling**: A method to jointly scale multiple model dimensions with a single coefficient φ.  
- **Neural Architecture Search (NAS)**: An automated way to design neural networks optimized for accuracy and efficiency.  
- **FLOPs**: Floating Point Operations — a measure of computational cost.

---

## ✅ TL;DR

EfficientNet presents a **scaling framework** that delivers high accuracy with low cost.  
By searching a compact baseline model and applying **balanced scaling**, it achieves **SOTA performance** with far fewer parameters and FLOPs than previous CNN architectures.  
Its ideas are simple yet powerful, and widely used in modern vision models and real-world deployments.

---

---

## ✅ Day 2 – Motivation Experiments & Compound Scaling

### 📌 Motivation by Experiments (Section 3)

EfficientNet experimentally evaluates how scaling only one dimension—depth, width, or resolution—affects model performance. The results clearly show **diminishing returns** when scaling is unbalanced.

#### 📊 Summary of Figure 2

| Subfigure | Experiment            | Summary |
|-----------|------------------------|---------|
| (a)       | Baseline               | EfficientNet-B0 from NAS |
| (b)       | Width only             | Early gains, but performance plateaus quickly |
| (c)       | Depth only             | Gradual improvement, but saturates after a point |
| (d)       | Resolution only        | Accuracy drops at very high input sizes due to noise/overfitting |

#### ⚠️ Key Insights

- **Depth**: Vanishing gradients, optimization challenges  
- **Width**: Better for fine-grained features, but lacks high-level semantics when overly wide  
- **Resolution**: Too large input → noise and computational overload

> ❗ Each single-axis scaling shows limited benefit → efficient scaling needs all three dimensions to grow **together**.

---

### 📌 Compound Scaling Formula (Section 4.1)

EfficientNet proposes **compound scaling**, a principled way to scale **depth, width, and resolution simultaneously**.

#### 🧮 Formula

$$
\text{depth} \propto \alpha^{\phi}, \quad 
\text{width} \propto \beta^{\phi}, \quad 
\text{resolution} \propto \gamma^{\phi}
$$

- **α**: depth factor  
- **β**: width factor  
- **γ**: resolution factor  
- **φ (phi)**: user-defined compound coefficient → how much to scale the model

#### 🔒 FLOPs Constraint

$$
\alpha \cdot \beta^2 \cdot \gamma^2 \approx 2
$$

- Ensures FLOPs double with each step φ → **computational budget grows steadily**
- β and γ are squared because width and resolution each affect FLOPs **quadratically**

#### 🧠 What φ Means

- φ = 0 → base model (EfficientNet-B0)  
- φ = 1 → depth ×α, width ×β, resolution ×γ  
- φ = 2 → depth ×α², etc.  
→ FLOPs grow roughly as: $$\text{FLOPs} \approx 2^{\phi}$$

---

### 💬 Quick Recap Q&A

- **Q: Why not just scale depth?**  
  A: Gradient vanishing, slow improvements

- **Q: Why not just scale resolution?**  
  A: Too large → noise, memory issues

- **Q: Why use α, β, γ?**  
  A: To balance scaling across model dimensions with fixed resources

---

