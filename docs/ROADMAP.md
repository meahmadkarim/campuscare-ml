# CampusCare ML Development Roadmap

**Status:** Actively Under Development

This roadmap outlines the milestones for the machine learning research and engineering phases of the project.

---

## Phase 1: Research and Problem Definition

### Goal
Establish the theoretical and methodological foundation for the classification system.

### Tasks
- [x] Define classification problem and department labels
- [x] Identify five primary department responsibilities
- [ ] Document ambiguity cases and boundary conditions
- [ ] Design dataset strategy and labeling instructions
- [ ] Define evaluation protocol and reproducibility standards
- [ ] Research alternative neural architectures for scratch training

**Deliverables:**
- Project Overview documentation
- Roadmap and methodology plan
- Labeling strategy document

---

## Phase 2: Dataset Development

### Goal
Collect, clean, and prepare the dataset for supervised learning.

### Tasks
- [ ] Obtain authorized or independently collected complaint examples
- [ ] Manually label examples following the established strategy
- [ ] Perform exploratory data analysis (EDA) and class balance check
- [ ] Create text preprocessing pipeline (tokenization, cleaning)
- [ ] Design leakage-resistant train/val/test splits
- [ ] Document dataset versioning and distribution stats

**Deliverables:**
- Labeled dataset (version 1.0)
- Preprocessing utilities
- EDA report

---

## Phase 3: Classical Machine Learning Baselines

### Goal
Establish strong performance baselines using statistical learning methods.

### Tasks
- [ ] Implement TF-IDF vectorization (unigrams, bigrams)
- [ ] Train and evaluate Multinomial Naive Bayes
- [ ] Train and evaluate Logistic Regression
- [ ] Train and evaluate Linear SVM
- [ ] Perform hyperparameter optimization for baselines
- [ ] Conduct detailed error analysis on baseline predictions

**Deliverables:**
- Baseline model artifacts
- Benchmark evaluation report
- Confusion matrices and error analysis insights

---

## Phase 4: Original Neural Network Development

### Goal
Design and train custom neural architectures from random initialization.

### Tasks
- [ ] Design character-level and word-level encoders
- [ ] Implement Bi-LSTM with attention from scratch
- [ ] Design and implement a small Transformer encoder (scratch)
- [ ] Train architectures on the text dataset
- [ ] Conduct hyperparameter search (learning rate, batch size, dropout)
- [ ] Compare performance against classical baselines

**Deliverables:**
- Custom neural model checkpoints
- Training logs and performance comparison
- Architecture design documentation

---

## Phase 5: Evaluation and Reliability

### Goal
Assess the reliability, calibration, and fairness of the trained models.

### Tasks
- [ ] Calculate macro/weighted F1-score and PR-AUC
- [ ] Generate and analyze confusion matrices per department
- [ ] Perform confidence calibration (Temperature scaling, Isotonic regression)
- [ ] Plot reliability diagrams (calibration curves)
- [ ] Evaluate model fairness across class distributions
- [ ] Design and test human-review confidence thresholds

**Deliverables:**
- Final evaluation report
- Calibrated confidence estimators
- Decision threshold recommendations

---

## Phase 6: Computer Vision Experiments

### Goal
Explore image-based classification for complaints with visual evidence.

### Tasks
- [ ] Collect and preprocess visual complaint photograph dataset
- [ ] Implement CNN baselines (ResNet-18, etc.)
- [ ] Train image classification models for department routing
- [ ] Evaluate image-only classification performance
- [ ] Compare visual features across different departments

**Deliverables:**
- Vision model checkpoints
- Visual classification performance report
- Preprocessing utilities for images

---

## Phase 7: Multimodal Learning

### Goal
Combine text and image features into a unified classification decision.

### Tasks
- [ ] Design multimodal fusion architectures (early vs. late fusion)
- [ ] Implement cross-modal attention mechanisms
- [ ] Train joint models on text-image pairs
- [ ] Conduct ablation studies (text vs. image vs. combined)
- [ ] Evaluate performance gains from multimodal learning

**Deliverables:**
- Multimodal model checkpoints
- Fusion architecture evaluation report
- Comparative analysis of unimodal vs. multimodal approaches

---

## Phase 8: Inference and Integration

### Goal
Deploy the models as a self-hosted API for the CampusCare web application.

### Tasks
- [ ] Implement model serialization and loading procedures
- [ ] Develop REST API using FastAPI or Flask
- [ ] Add input validation and error handling for inference
- [ ] Optimize model performance for low-latency requests
- [ ] Create Dockerized inference service
- [ ] Test integration with CampusCare backend

**Deliverables:**
- Inference API source code
- Docker image for model serving
- Integration tests and documentation

---

## Phase 9: Monitoring and Improvements

### Goal
Maintain and improve the model over time through monitoring and retraining.

### Tasks
- [ ] Design model versioning and artifact management system
- [ ] Implement prediction logging and feedback loops
- [ ] Establish monitoring for model drift and accuracy
- [ ] Create automated retraining pipeline
- [ ] Document procedures for model updates and rollbacks

**Deliverables:**
- Monitoring dashboard design
- Retraining schedule and procedures
- Model maintenance documentation

---
**Last Updated:** 2025-11-16