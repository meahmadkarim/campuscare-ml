# CampusCare ML

Independent machine learning research project for university complaint classification using NLP, neural networks, and multimodal learning.

[![Development Status: Actively Under Development](https://img.shields.io/badge/development_status-actively_under_development-yellow.svg)](https://github.com/meahmadkarim/campuscare-ml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python Version](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/pytorch-%23FF4F00.svg?style=for-the-badge&logo=pytorch&logoColor=white)](https://pytorch.org/)
[![GitHub issues](https://img.shields.io/github/issues/meahmadkarim/campuscare-ml)](https://github.com/meahmadkarim/campuscare-ml/issues)

---

## Project Overview

**CampusCare ML** is a standalone machine learning research and engineering project specializing in independently training classification models for routing university campus complaints to appropriate departments.

This is **not** a hosted foundation model service. No external AI inference APIs (GPT, Claude, Gemini, etc.) are used for complaint classification. The project develops custom neural networks trained from scratch, following reproducible scientific practices.

**Research Context:**
- **Goal:** Automatically classify English-language student complaints into one of five university departments (English only; no multilingual functionality)
- **Target Departments:**
  1. Maintenance & Facilities
  2. IT Department
  3. Security
  4. Academic Administration
  5. Student Affairs
- **Output:** Predicted department with confidence score
- **Fallback:** Human review for low-confidence predictions

**Relationship to CampusCare:**
CampusCare ML provides the classification models serving the [CampusCare web application](https://github.com/meahmadkarim/CampusCare). The ML system communicates via a self-hosted inference API deployed alongside CampusCare.

---

## Problem Statement

University campuses generate diverse student complaints and service requests that need to be routed to the correct department for response. Manual categorization is time-consuming, prone to bias, and creates bottlenecks in complaint resolution.

**Challenge:** Build reliable, explainable, and fair complaint classification models that can reliably distinguish between Maintenance/IT/Security/Academic/Student Affairs categories.

---

## Research Questions

1. How well can supervised text classification distinguish between academic and facilities complaints?
2. Which classical ML algorithms (TF-IDF + Naive Bayes/Logistic Regression/SVM) provide strong baselines?
3. Can original neural network architectures learn meaningful representations without pretrained embeddings?
4. How does confidence calibration perform across departments with different complaint volumes?
5. Can image-only classification complement text-based classification for visual complaints?
6. What fusion architecture (early, late, hybrid) works best for multimodal text-image learning?

---

## Objectives

Primary Research Objectives:

1. **Explore supervised multiclass text classification** for complaint routing
2. **Design and train original neural network architectures** from random initialization (no pretrained weights)
3. **Investigate statistical learning approaches** as strong baselines
4. **Evaluate confidence estimation and calibration** for human-in-the-loop workflows
5. **Test multimodal learning** for complaints with optional user-provided photographs
6. **Establish reproducibility** through consistent experimental protocols, open datasets, and model versioning

Secondary Objectives:

- Demonstrate fairness across departments with uneven complaint distributions
- Provide error analysis insights for manual review workflows
- Create a self-hosted inference API for CampusCare integration
- Document experimental procedures for future model retraining

---

## Classification Categories

The model predicts one primary department from these five classes:

1. **Maintenance & Facilities** — Building problems, utilities, furniture, physical infrastructure
2. **IT Department** — Hardware, software, network issues, technical assistance requests
3. **Security** — Campus safety, access control incidents, harassment, emergency situations
4. **Academic Administration** — Grades, examinations, attendance, course registration, transcripts
5. **Student Affairs** — Housing, scholarships, enrollment, records, peer support, grievances

**Additional Handling:**
- Low-confidence predictions (< threshold) trigger human review
- Out-of-domain text is tagged as "other" for human triage
- Ambiguous phrasing generates uncertainty signals

---

## Proposed Methodology

### Research Phases

The project follows a systematic ML lifecycle model with successive phases:

| Phase | Focus | Key Deliverables |
|-------|-------|------------------|
| **Phase 1** | Problem definition | Dataset strategy, label schema, ambiguity analysis |
| **Phase 2** | Dataset development | Curation, labeling, imbalance analysis, preprocessing |
| **Phase 3** | Classical ML baselines | TF-IDF, Naive Bayes, Logistic Regression, SVM metrics |
| **Phase 4** | Original neural networks | Design, train, evaluate custom architectures |
| **Phase 5** | Reliability assessment | Accuracy, precision, recall, calibration, error analysis |
| **Phase 6** | Computer vision experiments | Image preprocessing, CNN baselines, performance comparison |
| **Phase 7** | Multimodal learning | Text-image fusion designs, cross-modal attention |
| **Phase 8** | Inference system | Serialization, self-hosted API, CampusCare integration |
| **Phase 9** | Operations and updates | Versioning, monitoring, retraining schedule |

### Machine Learning Approaches to Investigate

#### 1. Classical Machine Learning (Phase 3)

- **Vectorization:** TF-IDF with bigrams, character n-grams, and word n-grams
- **Classifiers:**
  - Multinomial Naive Bayes
  - Logistic Regression
  - Linear Support Vector Machine (SVM)
- **Feature Engineering:** stop-word removal, stemming/lemmatization, subword tokenization evaluation

#### 2. Original Neural Network Architectures (Phase 4)

Research focus: **Design architecture from first principles, not copying pretrained models.**

- **Bidirectional LSTM/GRU** with attention mechanism
- **Transformer-based encoder** from scratch (scratch-built positional encodings, token embeddings)
- **Convolutional Neural Networks (CNN)** with parameter-efficient attention
- **Hybrid architectures** combining CNN feature extraction and LSTM sequential modeling
- **Context-aware embeddings** (character-level + word-level encoding)

**No pretrained model weights (BERT, GPT, Sentence-BERT, etc.) will be used.** All parameter initialization is random, subject to standard training procedures.

#### 3. Confidence Estimation (Phase 5)

- **Calibration techniques:** temperature scaling, isotonic regression
- **Uncertainty quantification:** Bayesian dropout (Monte Carlo), ensemble-based variance
- **Decision thresholds:** Per-department calibration for optimal human review rates

#### 4. Computer Vision (Phase 6)

- **Dataset design:** Visual complaint samples with associated labels
- **CNN architectures:** ResNet-18/34, EfficientNet small variants
- **Preprocessing:** standardization, augmentation (random rotation, brightness)
- **Image classification tasks:** Label prediction, multi-task (text + image)

#### 5. Multimodal Learning (Phase 7)

- **Architecture families:** early fusion (concatenate features), late fusion (vote/weight), hybrid fusion
- **Modalities:** text embeddings (optional TF-IDB), image features
- **Cross-modal attention:** learnable attention maps aligning text and image content
- **Ablation studies:** text-only performance, image-only performance, averaged multimodal scores

### Evaluation Metrics

| Metric | Purpose | Expected Analysis |
|--------|---------|--------------------|
| **Accuracy** | Overall performance, but sensitive to class imbalance | Report alongside weighted statistics |
| **Macro F1-Score** | Balance across classes, especially minority classes | Primary decision metric |
| **Weighted F1-Score** | Real-world utility considering complaint volumes | Compare with macro for imbalanced datasets |
| **Precision-Recall curves** | Evaluate tradeoffs per class | Class-PR curves and area under curve (PR-AUC) |
| **Confusion matrix** | Error patterns and department ambiguity | Visualize systematic confusions (e.g., IT vs. Security) |
| **Calibration curves** | Reliability of confidence scores | Calibration plots (reliability diagram) |
| **Error analysis** | Human-in-the-loop usefulness | Manually label and analyze edge cases |

---

## Dataset Strategy

### Data Requirements

#### Source Policy
- **Authorized dataset:** Only use complaint data with proper university data-donation or under Institutional Review Board (IRB) approved data collection
- **Independent collection:** Publicly available student complaint datasets from other universities (with consent)
- **Synthetic/simulated data:** Allowed for research prototypes, with limitations documented

#### Labeling Approach
- **Domain experts:** Departments designated as gold-standard labelers
- **Consensus labeling:** Multi-label review and conflict resolution
- **Ambiguity documentation:** Tag ambiguous cases for later analysis

#### Data Splits
- **Training (70-80%):** Training and hyperparameter tuning
- **Validation (10-15%):** Model selection, feature engineering
- **Test (10-15%):** Final evaluation on holdout data

#### Bias Mitigation
- **Class balancing:** Oversampling minority classes or using focal loss
- **Stratified sampling:** Ensure representative splits for each department
- **Temporal splits (if possible):** Avoid leaks between time periods

---

## Proposed Neural Network Architecture

> **Note:** This section describes planned investigations, not final architecture details.

### Text-Only Architecture (investigation focus)

```
Input: Raw complaint text (preprocessed)
  ↓
Char-level encoder (CNN or RNN) → Character embeddings
  ↓
Word-level encoder (embedding lookup) → Word embeddings
  ↓
Feature concatenation (optional: attention over words)
  ↓
Bi-directional LSTM/GRU → Contextual sequence representation
  ↓
Attention over sequence → Weighted aggregation
  ↓
Fully connected layer → Hidden representation
  ↓
Softmax classifier → 5-class probabilities
```

**Research Questions:**
- Should we build character-level CNNs early (capturing morphological info) or start from pretrained embeddings (if allowed)?
- How many attention heads improve classification without overfitting?
- Are deep LSTMs (2-3 layers) better than shallow GRUs with increased hidden dimension?
- Can we average multiple positions or aggregate across the most confident positions?

### Multimodal Architecture (Phase 7)

Two primary approaches to investigate:

**1. Modular Fusion (late fusion):**
- Train text model and image model separately
- Combine scores via weighted averaging or learned coefficients
- Pros: Easier inference, option for specialized optimization
- Cons: No cross-modal context learning

**2. Joint Fusion (early or hybrid):**
- Shared input embedding layer (optional)
- Multi-stream encoders (text branch, image branch) pretrained jointly
- Cross-attention module aligning representations
- Pros: Better alignment of modalities
- Cons: Less flexible, harder to interpret

### Parameter Count Investigations
- Small: < 10M parameters (efficient, fast training)
- Medium: 10-100M (balanced performance and cost)
- Large: > 100M (state-of-the-art accuracy, high compute cost)

---

## Model Evaluation Plan

### Baseline Comparisons

| Model | Features | Accuracy | Macro F1 | Parameters |
|-------|----------|----------|----------|------------|
| Multinomial Naive Bayes | TF-IDF | ? | ? | Small |
| Logistic Regression | TF-IDF | ? | ? | Medium |
| SVM (Linear) | TF-IDF | ? | ? | Medium |
| RGB (Bag-of-Words) | Word n-grams | ? | ? | Small |
| RNN-LSTM | Embeddings | ? | ? | Medium |
| Custom CNN | Char n-grams | ? | ? | Medium |
| Transformer (scratch) | Tokens | ? | ? | Large |

### Evaluation Protocol

1. **Offline Evaluation:** Train-test split with statistical significance testing (bootstrap sampling)
2. **Ablation Studies:** Remove attention, remove adjacencies, explore variants
3. **Error Analysis:** Sample misclassified examples, label manually, analyze patterns
4. **Calibration Testing:** Brier score, reliability diagram, spacing between sorted confidence bins
5. **Human Review Incentive:** Count false positives/negatives that human resolve vs. automated

---

## Proposed Technology Stack

### Implementation

- **Language:** 🟢 **Python 3.10+**
- **ML Framework:** 🟡 **PyTorch 2.0+** (primary, supports dynamic computation graphs suitable for research)
- **Data Manipulation:** 🟢 **pandas**, 🟢 **NumPy**
- **Text Processing:** 🟡 **scikit-learn** (for TF-IDF, baseline classifiers)
- **Preprocessing:** 🟡 **NLTK** or **spaCy** (tokenization, stemming/lemmatization)
- **Visualization:** 🟢 **Matplotlib**, 🟢 **seaborn**, 🟡 **plotly** (analysis and metrics)

### Development Tools

- **Git:** Version control
- **Jupyter Notebook/IPython:** Exploratory data analysis and model debugging
- **Weights & Biases** or **MLflow:** Experiment tracking (status: ⬜ TBD)
- **pytest/cov:** Testing framework (status: ⬜ TBD)

### Deployment (Future)

- **Model Serving:** 🟡 **FastAPI** or **Flask** (self-hosted inference API)
- **Containerization:** ⬜ TBD (Docker/Podman)
- **CI/CD:** ⬜ TBD (GitHub Actions)

---

## Development Roadmap

See `docs/ROADMAP.md` for detailed milestones.

### Summary Overview

**Phase 1: Research and Problem Definition**
- Define classification problem, establish department responsibilities
- Identify ambiguous complaint types, design dataset strategy
- Document equity considerations and boundary conditions

**Phase 2: Dataset Development**
- Obtain authorized or independently collected complaint examples
- Label and verify examples, inspect class balance
- Clean and preprocess data, create leakage-resistant splits
- Create dataset versioning and documentation

**Phase 3: Classical Machine Learning**
- TF-IDF feature vectorization
- Naive Bayes, Logistic Regression, Linear SVM baselines
- Benchmark evaluation and comparison with each other
- Error analysis to guide neural architecture design

**Phase 4: Original Neural Network**
- Learn and design an original text classification architecture from first principles
- Initialize parameters from random weights
- Train and validate the network, tune hyperparameters
- Compare performance with classical baselines

**Phase 5: Evaluation and Reliability**
- Primary metrics: accuracy, precision, recall, macro F1
- Visual analysis: confusion matrix, calibration curves
- Error analysis of misclassifications
- Confidence calibration and human-review strategy design

**Phase 6: Computer Vision**
- Photograph dataset preparation and preprocessing
- CNN architectures (ResNet/EfficientNet variants)
- Image classification experiments
- Compare image-only vs. text-only vs. combined performance

**Phase 7: Multimodal Learning**
- Text and image feature extraction pipelines
- Fusion architecture design (early/late/hybrid)
- Compare text-only, image-only, multimodal models
- Retrain model combining modalities with calibration

**Phase 8: Inference and Integration**
- Model serialization and loading procedures
- Self-hosted inference service (API)
- Integration with CampusCare backend
- Performance monitoring and latency testing

**Phase 9: Monitoring and Improvements**
- Model version management and artifact storage
- Feedback collection loop and retraining strategy
- Production performance monitoring (accuracy drift, latency)
- Reproducibility: document tools, seeds, hyperparameters

---

## Experimentation and Reproducibility Goals

### Reproducibility Standards

1. **Fixed Random Seeds:** torch.manual_seed(), numpy.random.seed() for all experiments
2. **Environment Tracking:** Use Dockerized environments or Conda environments with explicit dependencies
3. **Model Artifacts:** Save model checkpoints, tokenizer configs, training logs
4. **Code Sharing:** Submit research code to a public repository (this one)
5. **Documentation:** Document data schemas, preprocessing steps, model parameters

### Experiment Logging

Use experiment tracking to compare models systematically:

- Experiment name and description
- Data version and split configuration (train/val/test hashes)
- Model architecture and hyperparameters
- Training progress (iterations, epochs, loss curves)
- Evaluation metrics on validation and test sets
- Training time and resource usage (GPU/CPU hours)

---

## Local Inference and CampusCare Integration

### Self-Hosted Inference Interface

A lightweight REST API will be deployed to serve the trained model:

```
POST /classify
{
  "text": "The projector in classroom 3 is not working",
  "image": [optional base64-encoded image]
}

Response:
{
  "predicted_class": "IT Department",
  "predicted_id": 2,
  "confidence": 0.87,
  "all_scores": {
    "Maintenance & Facilities": 0.10,
    "IT Department": 0.87,
    "Security": 0.02,
    "Academic Administration": 0.01,
    "Student Affairs": 0.00
  }
}
```

### Integration Point

CampusCare backend will send complaint data to the CampusCare inference API:

- For text-only complaints: text field
- For multimodal complaints: text + image field
- Inference API returns class ID and confidence score
- CampusCare maps class ID to department name
- Low-confidence predictions routed to human admin review

**Security:** API protected with mutual TLS (mTLS) or API key authentication. No unencrypted HTTP.

**Scalability:** Model-fitted to GPU inference; CPU fallback with degraded performance for low-volume requests.

---

## Privacy and Responsible Data Use

### Ethical Principles

1. **Data Sovereignty:** Campus-specific complaint data remains within university and CampusCare infrastructure where possible
2. **Student Privacy:** Complainant anonymity preserved where applicable; no Personally Identifiable Information (PII) used for model training
3. **Transparency:** Public documentation of model limitations and failure modes
4. **Fairness:** Evaluate model performance across demographic or departmental groups
5. **Human Centered:** System reserves human review for critical and ambiguous decisions

### Data Governance

- **Informed Consent:** Dataset collection must respect GDPR, FERPA, and university data policies
- **Anonymization:** Remove PII (phone numbers, names) before training if feasible
- **Data Retention:** Clear policy on how complaint data is stored and how long models retain information

### Limitations and Omissions

- **No External APIs:** No GPT/Claude/Gemini calls; all classification is local, self-hosted, reproducible
- **Explainability:** Tool for viewing attention weights to explain model predictions (planned, not yet implemented)
- **Bias Monitoring:** Tracking which departments produce more misclassifications (ongoing)
- **Older Phones:** Images captured on low-resolution devices may impair visual classification

---

## Current Project Status

**Development Status:** Actively Under Development

**Phase 1: Research and Problem Definition**
- ✅ Classification problem defined
- ✅ Five department labels established
- ⬜ Dataset strategy design in progress
- ⬜ Ambiguity documentation plan

**Documentation Created:**
- [README.md](README.md) — Professional project overview
- [docs/PROJECT_OVERVIEW.md](docs/PROJECT_OVERVIEW.md) — Goals, scope, research questions
- [docs/ROADMAP.md](docs/ROADMAP.md) — Detailed development milestones
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — Proposed ML pipeline and inference architecture
- [docs/DATASET_STRATEGY.md](docs/DATASET_STRATEGY.md) — Planned sources, labels, privacy, splits
- [docs/EXPERIMENT_PLAN.md](docs/EXPERIMENT_PLAN.md) — Planned baseline and neural model comparisons

**Current Stage:** Research planning and toolchain preparation (Phase 1)

**What's Not Yet Done:**
- Data collection and labeling
- Training any ML models
- Running any experiments
- Implementing inference APIs

**Status clarification:**
- No models have been trained
- No datasets have been collected
- No experiments have been executed
- This repository presents the **planning** and **methodology**, not completed research

---

## Future Research Directions

Short-term:
1. **Dataset Acquisition:** Secure valid campus complaint data for supervised learning
2. **Baseline Implementation:** TF-IDF + Naive Bayes and Logistic Regression experiments
3. **First Neural Draft:** Train a simple RNN-LSTM or CNN model to learn representation patterns
4. **Calibration Studies:** Determine optimal confidence thresholds for human review

Medium-term:
5. **Architectural Research:** Compare 3-4 alternative architectures with systematic ablation
6. **Multimodal Experiments:** Evaluate text + image fusion for visual complaints
7. **Human Evaluation:** Conduct blind labeling contest comparing ML vs. human judgment
8. **Error Analysis Workshop:** Expert-led analysis of failure cases to improve data quality

Long-term:
9. **Active Learning:** Iterative model training with human-in-the-loop (select hard-to-classify items for labeling)
10. **Adaptability:** Progressive fine-tuning when prediction drifts
11. **Explainability Tools:** Add attention visualization and explanation generation
12. **Fairness Investigations:** Audit performance across demographics, use-case categories

---

## Link to CampusCare Web Application

The **[CampusCare](https://github.com/meahmadkarim/CampusCare)** web application will use this ML service internally for complaint routing:

- **Communication:** Self-hosted inference API following documented schemas
- **Data Flow:** Student complaint → classify → department assignment
- **Hybrid Mode:** AI prediction + human review fallback
- **No External APIs:** Classification is self-contained, no reliance on hosted LLMs

Visit the [CampusCare Repository](https://github.com/meahmadkarim/CampusCare) for full-stack application details.

---

## Contributing

**Roadmap:** As a research project, contributions should focus on:
- Improving documentation, methodology, and experimental protocols
- Suggesting better dataset strategies or fairness analyses
- Refining architecture designs and hyperparameter search approaches
- Opening issues that surface methodological questions

**Research Code:** When code is available:
- Follow the code style defined in CONTRIBUTING.md (in development)
- Write unit tests for reproducible utilities
- Share experiment logs and results transparently
- Open issues for collaborative debugging

---

## Citation

If this ML research informs your work, please cite:

```
CampusCare ML: Independent Machine Learning System for University Complaint Classification
Ahmad Karim, 2025
https://github.com/meahmadkarim/campuscare-ml
```

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

**© 2025 Ahmad Karim. Independent ML research for CampusCare.**