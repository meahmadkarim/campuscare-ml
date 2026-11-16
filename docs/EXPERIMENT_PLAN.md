# Experiment Plan: CampusCare ML

**Status:** Proposed Plan (Phase 1)

## Overview

This experiment plan defines how models will be evaluated, compared, and iterated upon during the research lifecycle.

## 1. Baseline Experiments (Phase 3)

### Objective
Establish performance benchmarks using classical machine learning algorithms.

### Configurations
- **Model 1:** Multinomial Naive Bayes + TF-IDF (unigrams + bigrams)
- **Model 2:** Logistic Regression + TF-IDF (C = [0.1, 1.0, 10.0])
- **Model 3:** Linear SVM + TF-IDF (hinge loss, L2 regularization)

### Evaluation Metrics
- Accuracy, Precision, Recall, Macro F1, Weighted F1.
- Training time and inference latency per sample.

## 2. Neural Network Experiments (Phase 4)

### Objective
Evaluate original neural network architectures trained from random initialization.

### Configurations
- **Model A:** Bi-LSTM with self-attention (Embedding dim: 128, Hidden dim: 256)
- **Model B:** Small Transformer encoder (Layers: 2, Heads: 4, Dim: 256)
- **Model C:** CNN with character n-grams

### Hyperparameter Search Space
- **Learning Rate:** `1e-4`, `5e-4`, `1e-3`
- **Batch Size:** `32`, `64`, `128`
- **Dropout Rate:** `0.1`, `0.3`, `0.5`
- **Optimizer:** AdamW, SGD with momentum

## 3. Multimodal Experiments (Phase 7)

### Objective
Determine the benefit of combining text and image modalities.

### Configurations
- **Baseline:** Text-only model (Best text model from Phase 4)
- **Baseline 2:** Image-only model (ResNet-18)
- **Experiment A:** Late fusion (weighted average of text and image probabilities)
- **Experiment B:** Early fusion (concatenating pooled text and image embeddings)

## 4. Tracking and Reproducibility

### Experiment Tracking
- Every experiment logs:
  - Hyperparameters and random seed
  - Training loss and validation loss per epoch
  - Validation metrics (Macro F1, Accuracy)
  - Confusion matrix visualization
  - Checkpoint path

### Reproducibility Standards
- Fixed seeds (`torch.manual_seed(42)`, `np.random.seed(42)`)
- Version-locked dependencies in `requirements.txt`
- Split hashes recorded in experiment logs

---
**Last Updated:** 2025-11-16