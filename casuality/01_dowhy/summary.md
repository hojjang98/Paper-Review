# 📄 Paper Summary: DoWhy

**Title**: DoWhy: An End-to-End Library for Causal Inference  
**Link**: [https://arxiv.org/abs/2011.04216](https://arxiv.org/abs/2011.04216)  
**Authors**: Amit Sharma, Emre Kiciman  
**Published by**: Microsoft Research, NeurIPS 2020 Workshop

---

## ✅ Day 1 – Motivation & Philosophical Foundations

### 📌 Why I Chose This Paper

In machine learning and data science, **explainable models** are increasingly crucial—not just for trust and transparency, but for real-world decision-making.  
However, most standard tools like linear regression, tree models, or neural networks only give us **correlations**, not **causal conclusions**.  
This paper addresses that exact gap by introducing **DoWhy**, a Python-based framework that provides a clear and testable structure for causal inference.

DoWhy appeals to me because:
- It doesn’t stop at statistical association—it pushes toward **true cause-effect understanding**  
- Its 4-step framework is intuitive yet rigorous  
- It bridges theory with practice through well-documented Python APIs  
- I can use it not just in toy examples, but in real-world evaluations (like treatment effect, policy modeling, behavior analysis)

---

### 📌 Abstract Summary

The authors present **DoWhy**, a library designed to support end-to-end **causal inference**, separating it into four steps:  
1) Modeling assumptions using causal graphs  
2) Identification using criteria like backdoor  
3) Estimation via statistical or ML models  
4) Refutation using falsifiability tests

This clear separation of responsibilities ensures more **robust and explainable causal estimates** than traditional methods.

---

### 📌 Introduction Insights

The paper opens with a powerful argument:  
> *"Correlation is not causation. Yet most data analyses stop at correlation."*

It identifies a core problem in data science:  
- Statistical tools often compute associations between variables  
- But rarely question **whether those relationships are causal**  
- And even when they do, the **assumptions behind those claims are implicit**, unverifiable, or forgotten

DoWhy tackles this by making all assumptions **explicit**, **testable**, and **reproducible** through a structured workflow.

---

### 📌 DoWhy's 4-Stage Framework

| Step | Purpose | Key Question |
|------|---------|--------------|
| **Model** | Define a causal graph | What assumptions do I believe about the world? |
| **Identify** | Check if the causal effect is mathematically identifiable | Can the effect be computed given the graph? |
| **Estimate** | Use data to estimate the effect | What’s the estimated magnitude of the effect? |
| **Refute** | Check robustness of the result | Could this result be due to bias or chance? |

---

### 📌 Confounders & Backdoor Criteria (Key Concepts)

- **Confounder**: A variable that affects both treatment and outcome  
  → e.g., age affecting both smoking and lung cancer  
- **Backdoor Criterion**: A graphical condition to block "spurious" paths in the causal graph  
  → Controlling for confounders ensures that only **true causal effects** are estimated

---

### 📌 3-Line Summary

- Most data analysis tools measure correlation, but DoWhy helps estimate true **causal relationships**  
- It achieves this through a 4-step pipeline: modeling, identification, estimation, and refutation  
- The framework encourages **explicit reasoning**, making results more robust and explainable

---

### 📌 Unfamiliar Terms

- **Identifiability**: Whether a causal quantity can be computed from the observed data  
- **Counterfactual**: A “what if” scenario used to test causal validity  
- **Refuter**: A test that checks whether the estimated effect holds under synthetic manipulation

---

> 📝 Notes:  
> This paper introduced a powerful mental model for thinking about causality.  
> Tomorrow I’ll dive into the implementation logic and real-world applications.

## ✅ Day 2 – Implementation & Applications

### 📌 DoWhy Pipeline in Code

DoWhy’s 4-stage pipeline (Model → Identify → Estimate → Refute) is implemented via the `CausalModel` class.  
It provides a structured interface for defining causal assumptions and testing them directly in Python.

The process is as follows:
1. **Modeling**: Define treatment, outcome, and confounders in a causal graph  
2. **Identification**: Verify if the effect can be estimated (e.g., via backdoor criterion)  
3. **Estimation**: Compute the causal effect using statistical or ML estimators  
4. **Refutation**: Test robustness through counterfactual simulations (e.g., placebo treatment)

---

### 📌 Example API Flow (No Code Here)

- You instantiate a `CausalModel` with your dataset and variables  
- Then, call `identify_effect()` to retrieve the estimand  
- Next, use `estimate_effect()` with a chosen method (like linear regression or matching)  
- Finally, test robustness with `refute_estimate()` using techniques like placebo or data subsetting

The entire pipeline encourages **explicit causal thinking**, even if your estimator is a simple regression model.

---

### 📊 Applications from the Paper

The authors showcase DoWhy through use cases like:
- **Medical**: Smoking → Lung cancer  
- **Education**: Online course participation → Exam performance  
- **Policy**: Program participation → Employment outcome

In each case, the framework enforces clear assumptions and encourages reproducibility by combining identification logic with statistical estimation and robustness testing.

---

### 📌 Notable Points

- DoWhy doesn’t replace causal reasoning — it **makes it executable**  
- The same causal pipeline can use different estimators (OLS, forest, IV, etc.)  
- Refutation is built-in, encouraging skepticism of any single result

---

### 🧠 Final Thoughts

This part of the paper showed that DoWhy is not just a library — it’s a **philosophy made runnable**.  
It rewards those who think structurally, and penalizes those who skip assumptions.

By separating **what we assume**, **what we compute**, and **what we test**,  
it provides a level of rigor that most ML pipelines still lack.

## ✅ Day 3 – Causal Estimation in Action (w/ Refutation Tests)

Today I tested the full DoWhy pipeline on a synthetic dataset.  
The goal was to compare a naive correlation-based estimate with a true causal estimate,  
and then assess the robustness of that estimate using placebo and subset refutation methods.

---

### 📌 Experimental Setup

- **Dataset**: Synthetic data with binary treatment, linear outcome, and 4 confounders  
- **True Causal Effect** (ground truth): set to β = 10  
- **Goal**: Estimate β using both naive regression and DoWhy, then evaluate robustness

---

### 📊 Step-by-Step Results

| Method | Estimated Effect | Notes |
|--------|------------------|-------|
| **Naive Linear Regression** | 15.48 | Overestimates due to unadjusted confounding |
| **DoWhy (Backdoor + Linear Regression)** | **9.9995** | Effect after adjusting for W0 ~ W3 |

---

### 🔍 Refutation Test Results

#### ✅ Refute: Placebo Treatment

```text
Estimated effect:      9.9995  
New effect (placebo):  0.0038  
p-value:               0.86

→ When the treatment is replaced with a random variable, the estimated effect vanishes.
This indicates the original causal effect was not due to chance — a strong positive sign.

✅ Refute: Subset of Data

Estimated effect:      9.9995  
New effect (subset):   9.9995  
p-value:               0.92

→ Even when using a random subset of the data, the effect remains virtually identical.
This suggests the effect is stable and generalizable across different samples.

📈 Visual Summary

![Effect Estimates Comparison](https://github.com/hojjang98/Paper-Review/blob/main/casuality/01_dowhy/do_why_graph.png?raw=true)  

Bar chart comparing Naive Regression, DoWhy Estimate, and two refutation tests.

🧠 Final Thoughts
This experiment showed how naive analysis can overestimate effects,
and how DoWhy enables more disciplined and robust causal inference.

Refutation results further support that the estimated effect is:

✅ Not spurious (Placebo test)

✅ Not sample-dependent (Subset test)






