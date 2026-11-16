# Dataset Strategy: CampusCare ML

**Status:** Proposed Strategy (Phase 1)

## Overview

This document outlines the dataset collection, curation, labeling, and splitting strategy for training and evaluating CampusCare ML models.

## 1. Data Sources and Acquisition

### Policy
- **No Private Data Compromise:** Data collection must comply with institutional policies and privacy standards.
- **Authorized Donation:** Compliant examples donated by departments or collected under IRB-approved protocols.
- **Synthetic Data:** Permitted for early research prototypes and edge-case testing, with clear tagging.

### Data Characteristics
- **Language:** English, Russian, and Kyrgyz (multilingual university environment).
- **Length:** Typically short text snippets (1-3 sentences) accompanied by optional photos.
- **Volume Target:** Minimum 1,000 labeled examples per department for initial baseline training (5,000 total).

## 2. Labeling Strategy

### Target Classes
1. **Maintenance & Facilities**
2. **IT Department**
3. **Security**
4. **Academic Administration**
5. **Student Affairs**

### Labeling Protocol
- **Expert Review:** Department heads and designated staff provide gold-standard labels.
- **Multi-Annotator Review:** Ambiguous samples labeled by at least two annotators.
- **Dispute Resolution:** In case of disagreement, a third senior reviewer makes the final classification.
- **Ambiguity Tagging:** Samples lacking clear departmental boundaries are labeled as `ambiguous` for human-review training.

## 3. Data Preprocessing Pipeline

### Text Pipeline
1. **Normalization:** Lowercasing, unicode normalization, removal of control characters.
2. **Tokenization:** Word-level and subword tokenization (BPE/WordPiece).
3. **Cleaning:** Removal of PII (names, phone numbers, email addresses, student IDs).
4. **Padding/Truncation:** Standard sequence length of 128-256 tokens.

### Image Pipeline
1. **Resize:** Scale images to standard dimension (e.g., 224x224 pixels).
2. **Normalization:** Zero-mean, unit-variance scaling based on ImageNet statistics.
3. **Augmentation (Training only):** Random horizontal flip, slight rotation, color jitter.

## 4. Dataset Splits

To prevent data leakage and evaluate generalization:

- **Training Set (70%):** Model parameter training.
- **Validation Set (15%):** Hyperparameter tuning, early stopping, and model selection.
- **Test Set (15%):** Final unbiased evaluation (never used during training or tuning).

### Stratification
Splits are stratified across departments to maintain consistent class distribution.

## 5. Privacy and Data Governance

- **PII Removal:** Automated regex and manual inspection to strip names, student IDs, and phone numbers.
- **Storage Encryption:** Raw datasets encrypted at rest on secure research volumes.
- **Access Control:** Restricted access to labeled datasets.

---
**Last Updated:** 2025-11-16