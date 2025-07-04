# 📄 Paper Summary: MobileNetV2

**Title**: MobileNetV2: Inverted Residuals and Linear Bottlenecks  
**Link**: [https://arxiv.org/abs/1801.04381](https://arxiv.org/abs/1801.04381)  
**Authors**: Mark Sandler, Andrew Howard, Menglong Zhu, Andrey Zhmoginov, Liang-Chieh Chen  
**Published by**: Google Research (2018)

---

## ✅ Day 1 – Overview & Purpose

### 📌 Abstract Summary
MobileNetV2 introduces a lightweight CNN architecture tailored for mobile and resource-constrained environments.  
Its core ideas—**Inverted Residuals** and **Linear Bottlenecks**—help reduce computation while maintaining representational power.  
It achieves strong performance on tasks like image classification, object detection, and semantic segmentation, with a balanced trade-off in accuracy, latency, and model size.

---

### 📌 Introduction Insights
The paper addresses the inefficiency of existing deep CNNs in mobile applications due to high computation and memory costs.  
To solve this, MobileNetV2 proposes a structure that maintains high accuracy while being computationally efficient.  
Its design expands low-dimensional features to high-dimensional ones, filters them using depthwise convolution, and projects them back linearly—avoiding large intermediate tensors during inference.

---

### 📌 Problem Statement
While modern deep networks achieve superhuman accuracy in tasks like image recognition, their resource demands exceed what mobile and embedded systems can handle.  
This paper proposes a new architecture optimized for such environments.  
The key module—**Inverted Residual with Linear Bottleneck**—expands inputs to high dimensions, filters them, and compresses back to low dimensions without incurring large memory costs, making it highly efficient for mobile inference.

---

### 📌 3-Line Summary
- Modern deep networks are accurate but unsuitable for mobile devices due to computation and memory constraints.  
- This paper introduces an efficient architecture that retains accuracy while minimizing resource usage.  
- The core design leverages Inverted Residuals and Linear Bottlenecks to reduce intermediate tensor size and computation.

---

### 📌 Unfamiliar Terms (None Today)

---

> 📝 Notes:  
> This summary is part of a structured review routine.  
> More in-depth architecture and experiments will be covered in Day 2.

## ✅ Day 2 – Core Architecture Breakdown

### 📌 Step 1: Depthwise Separable Convolution
To reduce computation, MobileNetV2 employs **depthwise separable convolutions**, splitting standard convolution into:
- **Depthwise Convolution**: One filter per input channel (spatial filtering).
- **Pointwise Convolution (1×1)**: Linear combinations across channels.

This reduces computation by a factor of **8–9×** (when using `3×3` kernels) with only a minor drop in accuracy, making it ideal for mobile devices.

---

### 📌 Step 2: Linear Bottlenecks
MobileNetV2 removes non-linearity (ReLU) after the final 1×1 projection layer in the bottleneck block.  
Why? Because **ReLU can destroy information** when applied to low-dimensional manifolds.  
By using a **linear projection**, the architecture preserves essential features and ensures representational capacity is not lost in compressed spaces.

---

### 📌 Step 3: Inverted Residuals
Contrary to traditional residual blocks (which connect wide layers), MobileNetV2 **connects narrow bottlenecks via shortcuts**.  
This inverted design:
- Avoids memory-heavy high-dimensional skips
- Preserves gradient flow
- Enables lightweight memory-efficient inference

Residual connections are only used when:
1. **Stride = 1**, and  
2. **Input and output dimensions match**

---

### 📌 Step 4: MobileNetV2 Block Structure

A single MobileNetV2 block follows this sequence:

```
Input → 1×1 Conv (Expansion, ReLU6)  
     → 3×3 Depthwise Conv (Stride s, ReLU6)  
     → 1×1 Conv (Projection, **Linear**)  
     → (Optional) Residual connection if stride=1 and input/output dims match
```

- **Expansion factor (t)** is typically `6`
- **ReLU6** is used for non-linearity due to its robustness on low-precision hardware
- The projection layer has no activation (Linear Bottleneck)

This structure is efficient, expressive, and well-suited for mobile inference.

---

### 📌 3-Line Summary
- MobileNetV2 splits convolution into efficient parts and carefully uses activation functions to preserve information.
- Inverted Residuals reduce memory and computation by skipping connections between compressed layers.
- Each block combines expansion, filtering, and projection in a lightweight, modular design.

---

### 📌 Unfamiliar Terms
- **Activation manifold**: A lower-dimensional surface where the activation values of a neural network lie.
- **ReLU6**: A capped ReLU activation function: `min(max(0, x), 6)`.

---

> 📝 Notes:  
> Day 3 will explore experimental results, ablations, and comparison with other models like MobileNetV1 and ShuffleNet.

## ✅ Day 3 – Experiments, Applications, and Conclusions

### 📌 Step 1: Architecture Details & Expansion

MobileNetV2 uses a repeating bottleneck block structure composed of:

```bash
1×1 Conv (Expansion, ReLU6)
→ 3×3 Depthwise Conv (Stride s, ReLU6)
→ 1×1 Conv (Linear Projection)
→ (+ Residual connection if stride = 1 and input/output dims match)
```

- Expansion factor `t` is typically set to **6**  
- Depthwise convolutions reduce computation drastically  
- Linear projection prevents information loss on low-dimensional manifolds  
- The network uses **19 such blocks** after an initial 3×3 Conv layer  
- **Dropout** and **Batch Normalization** are used during training  
- All kernels are 3×3; width multiplier and input resolution allow scaling

---

### 📌 Step 2: Experimental Results

#### 📍 Classification (ImageNet):
| Model | Top-1 Acc | MACs | Params |
|-------|-----------|------|--------|
| MobileNetV1 | 70.6% | 575M | 4.2M |
| **MobileNetV2** | **71.8%** | **300M** | **3.4M** |

- Despite using almost half the computation of V1, V2 achieves better accuracy

#### 📍 Object Detection (COCO, with SSDLite):
| Model | mAP | Latency |
|-------|-----|---------|
| MobileNetV1 + SSD | 19.3 | 27 ms |
| **MobileNetV2 + SSDLite** | **22.1** | **19 ms** |

- V2 achieves **higher accuracy and lower latency**

#### 📍 Memory Usage (Table 3):
- At all spatial resolutions, MobileNetV2 uses **the least memory** (Max = 400K)
- Ideal for low-memory mobile or edge devices

---

### 📌 Step 3: Real-World Applications

1. **SSDLite for Object Detection**  
   → Efficient mobile detector with better accuracy and lower latency  
2. **DeepLabv3 for Semantic Segmentation**  
   → Enables real-time segmentation with reduced computational load  
3. **Transfer Learning**  
   → Pretrained MobileNetV2 is widely used across CV tasks and mobile apps

---

### 📌 3-Line Summary

- MobileNetV2 delivers strong performance with minimal computation and memory usage.
- It improves upon V1 by introducing Inverted Residuals and Linear Bottlenecks.
- The architecture proves effective across classification, detection, and segmentation.

---

### 📌 Unfamiliar Terms
- **mAP (mean Average Precision)**: Measures detection accuracy across multiple IoU thresholds (COCO uses 0.5–0.95).
- **Width Multiplier**: A hyperparameter to scale model width (number of channels).
- **Materialized Memory**: Intermediate tensor storage needed during inference.

---

> 📝 Notes:  
> MobileNetV2 shows that with careful design, deep learning models can be both efficient and accurate.  
> Day 4 may explore successors like ShuffleNetV2 or EfficientNet.

## ✅ TL;DR

MobileNetV2 is a highly efficient architecture for mobile and edge devices.  
Its design — combining **Inverted Residuals** and **Linear Bottlenecks** — enables low-latency inference with minimal performance trade-offs.  
It outperforms its predecessor (MobileNetV1) across classification, detection, and segmentation tasks, all while cutting down memory and computation.

---

## 🛠️ Today's Implementation

- Built a **custom MobileNetV2 block** from scratch using PyTorch  
- Included key components:  
   Expansion (1×1 Conv + ReLU6)  
   Depthwise Conv (3×3 + ReLU6)  
   Projection (1×1 Linear Conv)  
   Optional residual connection when `stride=1` and in/out dims match  
- Verified the shape flow and block behavior with unit testing

> 📁 File: `MobileNetV2_Architecture_Implementation.ipynb`  
> 📂 Data dir: `./data/` (for future experiments)

---

## 🔜 Next Steps

- 🔍 **Explore MobileNetV3 and EfficientNet** for improvements over MobileNetV2  
- 🧪 **Reproduce training on a custom dataset** using V2 blocks  
- 🧱 **Implement and test variations**: change width multipliers, try ReLU vs. ReLU6, test without residuals  
- 🧠 **Compare with ShuffleNetV2** in terms of speed vs. accuracy trade-offs

---

## 💬 Final Thoughts

MobileNetV2 strikes a fine balance between performance and efficiency.  
For real-world, mobile-first CV applications, it remains a **gold standard** architecture.  
Going forward, I plan to **experiment with variants**, apply them to **custom datasets**, and explore **newer architectures** to deepen my practical understanding.





