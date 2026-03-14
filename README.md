# ML4SCI-E2E-Linear-Attention-Krrish-Kumar

### Tasks & Results
**Evaluation Metric:** **ROC-AUC** for particle classification and **RMSE** for mass regression. These metrics were selected to ensure the architecture effectively identifies particle types while maintaining high precision in reconstructing physical invariant mass.

---

### Task 1: Data Preprocessing & Pipeline
**Objective:** Transform raw CMS detector hits into a structured format suitable for high-resolution deep learning architectures.

**What I Did:**
* **Dataset Cleaning & Structuring:** Processed 8-channel detector data (ECAL, HCAL, Track $p_T$) into unified $125 \times 125$ resolution grids.
* **Target Derivation:** Calculated invariant mass targets from raw energy intensities to enable supervised regression tasks.
* **Normalization:** Implemented Z-score normalization and energy thresholds to handle the sparse nature of calorimeter data.
* **Preprocessing Checks:** Verified channel distributions across the 30GB unlabeled dataset to ensure no physical biases were introduced.

**Outcome:** Successfully established a high-throughput pipeline ready for both convolutional and transformer-based models.

---

### Task 2: Baseline Model Development
**Objective:** Establish a performance benchmark for End-to-End (E2E) tasks using standard deep learning approaches.

**What I Did:**
Initially, a Convolutional Neural Network (CNN) was implemented to serve as the baseline architecture for this project. This model was used to set a performance standard and to provide a reference point for evaluating the efficiency and accuracy of subsequent attention-based models. By training this baseline on the multi-channel detector images, I established a foundational benchmark for both classification and regression, allowing for a clear comparison of how local spatial filters perform relative to the global attention mechanisms introduced in later tasks.

---

### Task 3: Linear Attention & Performer Architectures
**Objective:** Implement $O(N)$ attention mechanisms to handle full-resolution detector images efficiently without memory bottlenecks.

**What I Did:**
* **Architectural Implementation:** Built **LinearViT** (using matrix associativity) and **PerformerViT** (using Orthogonal Random Features).
* **Multi-Task Training:** Trained models from scratch to evaluate their ability to capture long-range spatial correlations in $125 \times 125$ images.
* **Ablation Study:** Compared the two linear architectures to identify which best maintained physical accuracy while reducing computational complexity.

**Results:** **0.9275 AUC | 1.6211 RMSE (Performer Scratch)**

**Key Takeaways:** Global attention mechanisms proved significantly more effective for mass regression, providing high precision in energy reconstruction by capturing long-range dependencies across the detector grid.

---

### Task 4: Self-Supervised Pretraining & Finetuning
**Objective:** Leverage large-scale unlabeled data to boost model performance through transfer learning.

**What I Did:**
* **Pretraining Phase:** Trained the Linear and Performer backbones on 30GB of unlabeled CMS data to learn general jet representations without explicit labels.
* **Finetuning:** Loaded the pretrained weights and performed multi-task learning on labeled data for final classification and regression.
* **Comparison:** Benchmarked the finetuned models against the scratch-trained versions to quantify the performance gain from pretraining.

**Results:** **0.9303 AUC | 1.5238 RMSE (Finetuned)**

**Key Takeaways:** Finetuning on pretrained weights provided the most accurate results for mass regression (**RMSE 1.52**). The pretraining phase allowed the model to internalize deeper physical priors, leading to more stable and precise predictions than training from scratch.

---

### Model Performance Summary

| Model | Approach | Best AUC | Best RMSE |
| :--- | :--- | :--- | :--- |
| **LinearViT (Scratch)** | Linear Attention from Scratch | 0.9256 | 1.5759 |
| **PerformerViT (Scratch)** | Performer Attention from Scratch | 0.9275 | 1.6211 |
| **LinearViT (Finetuned)** | **Pretrained Linear Attention** | 0.9283 | **1.5238** |
| **PerformerViT (Finetuned)** | **Pretrained Performer Attention** | **0.9303** | 1.6205 |
