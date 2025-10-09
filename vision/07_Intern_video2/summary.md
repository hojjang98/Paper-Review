# 📄 Paper Summary: InternVideo2 – Scaling Video Foundation Models for Multimodal Understanding

**Title**: InternVideo2: Scaling Video Foundation Models for Multimodal Understanding  
**Authors**: Yujie Zhong, Yinan He, Jinghao Zhou, et al.  
**Published in**: CVPR 2024 (Best Paper Honorable Mention)  
**Link**: [https://arxiv.org/abs/2403.15377](https://arxiv.org/abs/2403.15377)  

---

## ✅ Day 1 – Abstract & Introduction  

### 📌 Background & Motivation  
- Vision-language foundation models like **CLIP**, **BLIP-2**, and **InternVideo (v1)** have achieved great success in bridging visual and textual modalities.  
- However, **videos** pose unique challenges beyond static images:  
  - Temporal dependencies (motion, dynamics)  
  - Long-range context and multi-modal interactions (visual, audio, speech)  
- Existing models often rely on **separate objectives** (contrastive, masked modeling, captioning) without a unified framework.  
- There is a strong need for a **scalable video foundation model** that can integrate these training paradigms effectively.

---

### 📌 Core Idea  
InternVideo2 proposes a **progressive multimodal training framework** that combines:  
1. **Masked video modeling (MVM)** – learn spatiotemporal representations by reconstructing masked video tokens.  
2. **Multimodal contrastive learning** – align video, text, audio, and speech features.  
3. **Next-token prediction** – enable contextual understanding and generation capabilities.  

This unified design allows the model to learn both **temporal coherence** and **cross-modal alignment** across large-scale data.

---

### 📌 Model Highlights  
- **Unified architecture** integrating spatial-temporal modeling and multimodal fusion.  
- **Scalable pretraining** on 400M video-text pairs from diverse sources.  
- **Hierarchical temporal encoder** for long video understanding.  
- Trained progressively from base → giant (up to **6B parameters**).  
- Supports **video retrieval**, **captioning**, **action recognition**, and **video dialogue** tasks.  

---

### 📌 Training Strategy  
- Employs a **progressive curriculum**:  
  1. Start with masked video pretraining for spatial-temporal consistency.  
  2. Add multimodal contrastive alignment (video–text–audio–speech).  
  3. Conclude with autoregressive next-token prediction for reasoning.  
- Leverages **semantic video segmentation** and **caption fusion** to ensure tight alignment between modalities.  
- Uses large-scale distributed training (DeepSpeed, FlashAttention) for efficiency.  

---

### 📌 Main Contributions  
1. Introduces a **unified multimodal video-language foundation model** trained on massive, diverse datasets.  
2. Proposes a **progressive training framework** combining reconstruction, contrastive, and generative objectives.  
3. Demonstrates **state-of-the-art results** on 60+ benchmarks covering retrieval, recognition, and dialogue tasks.  
4. Shows strong **scalability** and **generalization** to long video and audio-visual reasoning.  

---

### 📌 Key Observations  
- Scaling video foundation models yields consistent performance gains, mirroring trends in LLMs and CLIP-like systems.  
- Multimodal supervision (especially audio and speech) greatly enhances semantic understanding.  
- The framework generalizes across domains without task-specific finetuning.  

---

### 📌 TL;DR  
InternVideo2 = Unified, scalable, and multimodal video foundation model.  
Trained progressively via masked modeling + contrastive alignment + next-token prediction,  
it achieves SOTA across a wide range of video-language tasks.  

---


## ✅ Day 2 – Architecture & Method  

### 📌 Overview  
InternVideo2 builds upon InternVideo (v1) but introduces a **unified spatial–temporal–multimodal architecture** capable of handling visual, textual, and auditory inputs within a single framework.  
The architecture is composed of four major components:  

1. **Spatial-Temporal Backbone (ST-Backbone)**  
2. **Cross-Modal Fusion Module (CMFM)**  
3. **Temporal Modeling Layer**  
4. **Multitask Pretraining Heads**

---

### 🧩 1. Spatial–Temporal Backbone  
- Based on **ViT/TimeSformer-like architecture**, extended to handle spatiotemporal tokens.  
- Each video clip is divided into **3D patches** (spatial + temporal cubes).  
- The model applies **spatial attention within each frame**, and **temporal attention** across frames.  
- Enables efficient long-range temporal modeling through **factorized attention** (space × time).  

\[
x_{t} = \text{TemporalAttention}(\text{SpatialAttention}(x_{t-1}))
\]

This design preserves local motion while maintaining global consistency across video sequences.  

---

### 🔄 2. Cross-Modal Fusion Module (CMFM)  
- Key innovation: allows **video, text, audio, and speech embeddings** to interact.  
- Uses **multi-modal attention blocks**, where each modality can attend to the others via **shared latent space**.  
- Implements **gated fusion** to control cross-modal influence dynamically.  

\[
z = \text{Gate}(\text{Attention}(V, T, A, S))
\]

where \(V, T, A, S\) are video, text, audio, and speech embeddings, respectively.  

**Purpose:** builds rich semantic alignment across modalities — crucial for tasks like captioning and dialogue.  

---

### 🕒 3. Temporal Modeling Strategy  
- Introduces a **Hierarchical Temporal Encoder (HTE)** for understanding long-range motion.  
- Videos are chunked into temporal windows → encoded → aggregated at higher levels.  
- This structure scales efficiently to **long videos** without quadratic attention cost.  
- Similar in spirit to **Temporal Pyramid Networks**, but integrated into the backbone.  

---

### 🎯 4. Multi-Task Pretraining Heads  
Each pretraining task connects to the shared backbone via lightweight task heads:  

| Task | Objective | Description |
|------|------------|-------------|
| **Masked Video Modeling (MVM)** | Reconstruction | Predict masked patches → robust spatiotemporal representation |
| **Contrastive Learning** | Alignment | Match video ↔ text ↔ audio ↔ speech embeddings |
| **Next-Token Prediction** | Generation | Enable multimodal reasoning & video captioning |
| **Action Classification** | Supervised | Optional task for temporal discrimination |

These tasks are **trained progressively** rather than jointly to stabilize optimization.

---

### ⚙️ Training Flow Summary  
1. **Stage 1 – MVM:** Learn general motion & appearance understanding.  
2. **Stage 2 – Contrastive:** Align multimodal representations.  
3. **Stage 3 – Next-token prediction:** Learn generative reasoning and dialogue ability.  

---

### 📌 Key Takeaways  
- Unified architecture merges **space, time, and modality** within one Transformer-based backbone.  
- **Progressive training** helps the model move from perception → alignment → reasoning.  
- Enables scalability up to **6B parameters** while maintaining efficiency via hierarchical design.  

---


