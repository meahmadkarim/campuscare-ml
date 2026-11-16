# Project Overview: CampusCare ML

**Status:** Actively Under Development

## Research Goals

The primary goal of **CampusCare ML** is to design, implement, and evaluate an independent machine learning system capable of accurately routing university student complaints to the appropriate administrative department.

The research investigates the feasibility of **scratch-trained neural networks** for domain-specific text classification without reliance on external hosted foundation models (GPT, Claude, Gemini, etc.).

### Primary Objectives
1. **Develop a robust classification engine** for five university departments: Maintenance, IT, Security, Academic Administration, and Student Affairs.
2. **Design original neural network architectures** that learn meaningful representations from randomly initialized weights.
3. **Compare performance** with classical statistical learning baselines (TF-IDF + Naive Bayes/SVM).
4. **Evaluate confidence calibration** to enable reliable human-in-the-loop workflows.
5. **Investigate multimodal learning** by incorporating student-provided photographs into the classification decision.

## Research Questions

1. **Classification Accuracy:** Can a custom-trained model achieve high macro F1-scores across departments with varying complaint volumes?
2. **Architecture Impact:** Do Transformer encoders from scratch outperform Bi-LSTM with attention for short, informal student complaint text?
3. **Dataset Sensitivity:** How many labeled examples per department are required to reach an acceptable baseline performance?
4. **Multimodal Fusion:** Does adding visual features from photographs significantly reduce prediction error compared to text-only models?
5. **Human-AI Collaboration:** What is the optimal confidence threshold for routing uncertain cases to human review while minimizing administrative burden?

## Scope

### In-Scope
- Text preprocessing and exploratory data analysis (EDA).
- Classical ML classification experiments.
- Neural network architecture design, training, and evaluation.
- Computer vision experiments for visual complaints.
- Multimodal text-image fusion research.
- Confidence calibration and reliability assessment.
- Self-hosted inference API development.

### Out-of-Scope
- Use of hosted LLM APIs (OpenAI, Anthropic, Google, etc.).
- Fine-tuning of extremely large pretrained models (BERT, GPT).
- Production deployment at a university-wide scale.
- Collection of personally identifiable information (PII).
- Complaint resolution or ticketing system logic (handled by CampusCare).

## Target Audience
- Machine learning researchers and students interested in NLP and computer vision.
- University administrators exploring automation in student services.
- Developers looking for self-hosted classification alternatives to hosted APIs.

## Success Criteria
- **Baseline:** Macro F1-score > 0.70 on holdout set.
- **Goal:** Macro F1-score > 0.85 with multimodal fusion.
- **Reliability:** Calibration error (ECE) < 10% for confidence scores.
- **Integration:** Latency < 500ms for inference requests.
- **Reproducibility:** Fully documented code, hyperparameters, and dataset schemas.

---
**Last Updated:** 2025-11-16