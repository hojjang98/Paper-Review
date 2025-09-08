# 📄 Paper Summary: Attention Is All You Need

**Title**: Attention Is All You Need  
**Link**: [https://arxiv.org/abs/1706.03762](https://arxiv.org/abs/1706.03762)  
**Published in**: NeurIPS 2017  

---

## ✅ Day 1 – Abstract & Introduction  

### 📌 Background & Motivation  
- Traditional **sequence transduction models** mainly relied on **RNNs/LSTMs/GRUs** or **CNNs**.  
- These models typically adopt an **encoder–decoder architecture**, often enhanced with **attention mechanisms**.  
- However, recurrent models suffer from:  
  - **Sequential computation** → no parallelization possible  
  - **Inefficiency** with long sequences (memory bottlenecks, slower training)  
  - High training cost and limited scalability  

---

### 📌 Proposed Approach  
- Introduces the **Transformer**, a new architecture:  
  - **Removes recurrence and convolution entirely**  
  - Relies **solely on attention mechanisms** for sequence modeling  
- Key benefits:  
  - Enables **parallelization across sequence elements**  
  - Reduces training time significantly  
  - Captures **long-range dependencies** more effectively  

---

### 📌 Key Contributions  
1. **Attention-only architecture** replacing RNNs and CNNs  
2. **Parallelizable computation**, boosting efficiency on GPUs  
3. **State-of-the-art translation performance**:  
   - WMT 2014 English→German: 28.4 BLEU (+2 BLEU improvement)  
   - WMT 2014 English→French: 41.8 BLEU (new single-model SOTA)  
4. **Generalization beyond translation**, e.g., English constituency parsing  

---

### 📌 High-Level Workflow  
1. **Traditional models**: sequential hidden state updates → bottlenecked by recurrence  
2. **Problem**: sequential nature blocks parallelization, slows training  
3. **Transformer**: uses attention to select relevant words from the entire input at once  
4. **Outcome**: faster training, better performance, improved generalization  

---

### 📌 Early Results  
- Achieved SOTA with only **12 hours of training on 8 P100 GPUs**  
- Training cost much lower than prior architectures  
- Proves that **attention-only design is both efficient and powerful**  

---

### 📌 TL;DR Summary  
The Transformer replaces recurrence and convolutions with **pure attention mechanisms**,  
achieving **parallelization, faster training, and superior accuracy** across translation and other NLP tasks.  

---

## ✅ Day 2 – Encoder–Decoder & Scaled Dot-Product Attention  

### 📌 Encoder–Decoder Architecture  
- Most sequence transduction models adopt an **encoder–decoder structure**.  
- **Encoder**: maps input sequence  
  $$
  (x_1, …, x_n)
  $$  
  into continuous representations  
  $$
  z = (z_1, …, z_n).
  $$  
- **Decoder**: generates output sequence  
  $$
  (y_1, …, y_m)
  $$  
  autoregressively, using previously generated tokens as additional input at each step.  

---

### 📌 Transformer Encoder  
- Composed of **N = 6 identical layers**.  
- Each layer has two sub-layers:  
  1. Multi-Head Self-Attention  
  2. Position-wise Feed-Forward Network  
- Each sub-layer uses **Residual Connection** + **Layer Normalization**:  
  $$
  output = LayerNorm(x + Sublayer(x))
  $$  
- All outputs, including embeddings, share dimension  
  $$
  d_{model} = 512.
  $$  

---

### 📌 Transformer Decoder  
- Also composed of **N = 6 identical layers**.  
- Each layer has three sub-layers:  
  1. Masked Multi-Head Self-Attention  
  2. Encoder–Decoder Attention  
  3. Position-wise Feed-Forward Network  
- Key points:  
  - Decoder self-attention applies **masking** to block future positions.  
  - Output embeddings are **offset by one position** to ensure predictions depend only on past outputs.  

---

### 📌 Attention: General Definition  
- An **attention function** maps a query and a set of key–value pairs to an output.  
- The output is a **weighted sum of values**, where weights are determined by a compatibility function between query and keys.  

---

### 📌 Scaled Dot-Product Attention  
- Inputs:  
  - Queries and Keys of dimension  
    $$
    d_k
    $$  
  - Values of dimension  
    $$
    d_v
    $$  
- Process:  
  1. Compute dot products between Query and all Keys  
  2. Scale by  
     $$
     \sqrt{d_k}
     $$  
  3. Apply **softmax** to obtain weights  
  4. Multiply weights with Values and sum  
- Formula:  
  $$
  Attention(Q, K, V) = softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
  $$  

---

### 📌 Additive vs Dot-Product Attention  
- **Additive Attention**: uses a small feed-forward network for compatibility.  
- **Dot-Product Attention**: uses dot products directly.  
- Complexity is similar, but dot-product is much faster and more memory-efficient due to optimized matrix multiplications.  
- For large  
  $$
  d_k
  $$  
  scaling is crucial to prevent vanishing gradients in softmax.  

---

### 📌 Key Takeaways (Day 2)  
1. Transformer retains an **encoder–decoder structure**, but each block relies solely on self-attention and feed-forward networks.  
2. Encoder and Decoder employ **Residual Connections + LayerNorm** for stable training.  
3. Attention is essentially **Q–K similarity guiding weighted V aggregation**.  
4. Scaled Dot-Product Attention ensures both **computational efficiency** and **training stability**.  

---

👉 Next (Day 3): Multi-Head Attention – extending Scaled Dot-Product Attention with multiple parallel heads.  
