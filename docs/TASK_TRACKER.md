# CampusCare-ML — Central Multi-Agent Task Tracker

This document is the single authoritative development task tracker for **CampusCare-ML**. It coordinates work across **OpenAI Codex** (`agent/codex`), **Anthropic Claude Code** (`agent/claude`), and **Google Antigravity** (`agent/antigravity`), as well as human project maintainers.

> **Relationship to Existing Planning Notes:** Existing files in `docs/` (`PROJECT_OVERVIEW.md`, `ARCHITECTURE.md`, `DATASET_STRATEGY.md`, `EXPERIMENT_PLAN.md`, and `ROADMAP.md`) represent preliminary conceptual notes from repository initialization. No machine learning pipelines, datasets, models, or inference services have been implemented yet. This tracker (`docs/TASK_TRACKER.md`) is the authoritative operational record of task status and ownership.

---

## 1. Tracker Governance and Rules

### 1.1 Supported Task Statuses

Every task in this tracker must use one of the following six exact status values:

| Status | Definition | Entry Criteria |
|---|---|---|
| **Backlog** | Planned future work not yet eligible for execution. | Task is defined, but upstream dependencies, specifications, or approvals are not yet complete. |
| **Ready** | Eligible to be started immediately by the assigned owner. | All upstream task dependencies are `Completed` and required specifications/approvals are in place. |
| **In Progress** | Actively being worked on by the assigned primary agent. | Agent has verified its branch/worktree (`agent/<agent>`) and begun implementation or documentation. |
| **Blocked** | Execution is halted and cannot proceed safely. | Missing specification, unresolved interface conflict, failing upstream dependency, or awaiting user authorization. |
| **In Review** | Implementation and self-verification are complete; awaiting review. | All task deliverables and verification checks are recorded; awaiting peer agent review (typically Claude Code) and/or user approval. |
| **Completed** | Formally verified, reviewed, and approved/integrated. | All acceptance criteria are demonstrably satisfied, review has passed, and changes are approved by the user. |

**Strict Completion Rule:** An agent must **never** mark a task `Completed` simply because code or documentation has been drafted. Marking a task `Completed` requires that all agreed acceptance criteria are verified with real evidence and reviewed where applicable.

---

### 1.2 Priority Levels

Every task is assigned one of four priority levels:

| Priority | Definition | How Priority Is Determined |
|---|---|---|
| **Critical** | Foundational infrastructure, safety/governance controls, or immediate path blockers. | Blocks all or most downstream work across agents, or establishes mandatory safety, reproducibility, and Git isolation rules. |
| **High** | Primary phase deliverables on the critical path for the active milestone. | Directly required to complete the current project phase or to prevent statistical/architectural defects (e.g., data leakage, schema design). |
| **Medium** | Standard engineering, evaluation, or developer-experience tasks. | Adds essential project capabilities, tests, or visualizations but does not immediately block parallel foundational tasks. |
| **Low** | Secondary enhancements, exploratory extensions, or optional tooling. | Can be deferred without impacting core baseline reproducibility, model correctness, or release readiness. |

---

### 1.3 Task Assignment and Ownership Rules

1. **Single Primary Owner:** Every task has exactly one responsible primary agent (`Codex`, `Claude Code`, or `Antigravity`) operating in its dedicated branch (`agent/codex`, `agent/claude`, or `agent/antigravity`) or on `main` when explicitly authorized for repository baseline setup.
2. **Reviewers and Collaborators:** Other agents may act as reviewers or collaborators, but must never edit the primary owner's worktree directly.
3. **No Simultaneous Duplicate Ownership:** Never assign the same implementation task to multiple agents concurrently.
4. **Task Decomposition:** When an initiative spans multiple roles (for example, Codex implementing an evaluation module, Claude Code auditing statistical correctness, and Antigravity building diagnostic plots), decompose it into separate tasks with explicit task IDs and dependencies.
5. **Task ID Prefixes:**
   - `INFRA-xxx`: Repository, workspace, worktree, and collaboration infrastructure
   - `ARCH-xxx`: Problem formulation, system architecture, contracts, and methodology
   - `DATA-xxx`: Dataset sourcing, validation, cleaning, labeling, and splitting
   - `MODEL-xxx`: Classical ML, neural networks, calibration, serialization, and inference
   - `TEST-xxx`: Unit/integration testing, evaluation verification, leakage audits, and QA
   - `DOC-xxx`: Shared instructions, visualizations, inspection UIs, and technical documentation

---

## 2. Summary Dashboard

| Phase | Phase Name | Total Tasks | Completed | In Review | In Progress | Blocked | Ready | Backlog |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| **Phase 0** | Development Infrastructure | 9 | 8 | 0 | 0 | 0 | 1 | 0 |
| **Phase 1** | Project Research and Architecture | 5 | 0 | 0 | 0 | 0 | 0 | 5 |
| **Phase 2** | Dataset Engineering | 5 | 0 | 0 | 0 | 0 | 0 | 5 |
| **Phase 3** | Baseline Machine Learning | 5 | 0 | 0 | 0 | 0 | 0 | 5 |
| **Phase 4** | Advanced Machine Learning | 5 | 0 | 0 | 0 | 0 | 0 | 5 |
| **Phase 5** | Local Inference and Testing | 5 | 0 | 0 | 0 | 0 | 0 | 5 |
| **Phase 6** | Documentation and Release Preparation | 4 | 0 | 0 | 0 | 0 | 0 | 4 |
| **Total** | **All Phases** | **38** | **8** | **0** | **0** | **0** | **1** | **29** |

---

## 3. Detailed Task Registry by Phase

### Phase 0 — Development Infrastructure

#### `INFRA-001` — Git Repository Initialization and Remote Baseline Setup
- **Task ID:** `INFRA-001`
- **Task Title:** Git Repository Initialization and Remote Baseline Setup
- **Description:** Initialize and clone the standalone CampusCare-ML Git repository, verify the `main` branch and `origin` remote configuration, and establish initial planning notes.
- **Responsible Agent:** Google Antigravity (Verified)
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** None
- **Assigned Git Branch:** `main`
- **Acceptance Criteria:**
  - Repository root `C:\Users\1000014573\My_Projects\CampusCare-ML` is a valid Git repository on branch `main`.
  - Remote `origin` tracks the standalone CampusCare-ML GitHub repository.
- **Related Commit / PR:** `651ab88` (`docs: improve project README and development roadmap`), `63f17e5` (`chore: add CampusCare-ML VS Code workspace`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-002` — Root `.gitignore` and Multi-Agent Worktree Isolation Rules
- **Task ID:** `INFRA-002`
- **Task Title:** Root `.gitignore` and Multi-Agent Worktree Isolation Rules
- **Description:** Configure `.gitignore` with Python cache, virtual environment, secret/env, testing, temporary artifact, selective `.vscode/*`, and mandatory `/.worktrees/` ignore rules.
- **Responsible Agent:** Google Antigravity
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** `INFRA-001`
- **Assigned Git Branch:** `main`
- **Acceptance Criteria:**
  - `.gitignore` excludes Python bytecode, virtual environments, secrets, test caches, temporary logs, and `/.worktrees/`.
  - `git check-ignore -v .worktrees/codex/example.txt` confirms `.worktrees/` is ignored by `main`.
  - `.code-workspace` and `.env.example`/`.env.sample` remain un-ignored.
- **Related Commit / PR:** `9f23021` (`chore: configure multi-agent workspace infrastructure`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-003` — Isolated Agent Git Worktrees (`agent/codex`, `agent/claude`, `agent/antigravity`)
- **Task ID:** `INFRA-003`
- **Task Title:** Isolated Agent Git Worktrees (`agent/codex`, `agent/claude`, `agent/antigravity`)
- **Description:** Create three dedicated Git worktrees under `.worktrees/codex`, `.worktrees/claude`, and `.worktrees/antigravity` checked out on branches `agent/codex`, `agent/claude`, and `agent/antigravity` from the verified `main` baseline.
- **Responsible Agent:** Google Antigravity
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** `INFRA-002`
- **Assigned Git Branch:** `main`
- **Acceptance Criteria:**
  - `git worktree list` recognizes `main` and all three agent worktrees.
  - Each worktree is checked out on its unique assigned branch with a clean working tree.
- **Related Commit / PR:** `9f23021` (`chore: configure multi-agent workspace infrastructure`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-004` — Multi-Root VS Code Workspace Configuration (`CampusCare-ML.code-workspace`)
- **Task ID:** `INFRA-004`
- **Task Title:** Multi-Root VS Code Workspace Configuration (`CampusCare-ML.code-workspace`)
- **Description:** Configure `CampusCare-ML.code-workspace` to expose all four workspace roots (`Main`, `Codex`, `Claude`, `Antigravity`) and safe editor, Python, PowerShell terminal, file exclusion, and Git settings.
- **Responsible Agent:** Google Antigravity
- **Priority:** High
- **Current Status:** Completed
- **Dependencies:** `INFRA-003`
- **Assigned Git Branch:** `main`
- **Acceptance Criteria:**
  - `CampusCare-ML.code-workspace` parses as valid JSON and resolves all four workspace root paths.
  - Settings preserve worktree visibility and disable automatic Git commits/fetches.
- **Related Commit / PR:** `b6c5216` (`chore: configure isolated agent worktrees and VS Code workspace`)
- **Last Updated Date:** 2026-11-17

#### `DOC-001` — Shared Multi-Agent Development Instructions (`AGENTS.md`)
- **Task ID:** `DOC-001`
- **Task Title:** Shared Multi-Agent Development Instructions (`AGENTS.md`)
- **Description:** Draft and verify the central `AGENTS.md` file in the repository root defining project identity, agent roles, Git/worktree rules, Python and ML standards, collaboration workflow, review rules, handoff principles, conflict resolution, and safety boundaries.
- **Responsible Agent:** Google Antigravity
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** `INFRA-004`
- **Assigned Git Branch:** `main`
- **Acceptance Criteria:**
  - `AGENTS.md` exists in the repository root and includes all 10 mandatory policy sections.
  - Reviewed and approved by the user before committing to `main`.
- **Related Commit / PR:** `877e557` (`docs: add shared multi-agent development instructions`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-005` — Central Multi-Agent Task Tracker (`docs/TASK_TRACKER.md`)
- **Task ID:** `INFRA-005`
- **Task Title:** Central Multi-Agent Task Tracker (`docs/TASK_TRACKER.md`)
- **Description:** Create the authoritative multi-agent task tracker covering Phase 0 through Phase 6 with standardized task IDs, statuses, priorities, ownership rules, dependencies, and acceptance criteria.
- **Responsible Agent:** Google Antigravity
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** `DOC-001`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - `docs/TASK_TRACKER.md` exists in the Antigravity worktree and defines all required fields, status rules, priority rules, and phase tasks (`Phase 0`–`Phase 6`).
  - No unimplemented ML tasks are falsely marked `Completed`.
- **Related Commit / PR:** `63b125c` (`docs: add multi-agent task tracker, handoff system, and git workflow`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-006` — Multi-Agent Handoff Protocol and Template (`docs/handoffs/`)
- **Task ID:** `INFRA-006`
- **Task Title:** Multi-Agent Handoff Protocol and Template (`docs/handoffs/`)
- **Description:** Create `docs/handoffs/README.md` and `docs/handoffs/HANDOFF_TEMPLATE.md` defining normal handoffs, emergency handoffs, worktree-safe recovery procedures, naming conventions, and quality standards.
- **Responsible Agent:** Google Antigravity
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** `DOC-001`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - `docs/handoffs/HANDOFF_TEMPLATE.md` includes sections A through J.
  - `docs/handoffs/README.md` documents both the 10-step normal handoff workflow and the 8-step emergency recovery workflow without violating worktree isolation.
- **Related Commit / PR:** `63b125c` (`docs: add multi-agent task tracker, handoff system, and git workflow`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-007` — Multi-Agent Git Collaboration Workflow (`docs/GIT_WORKFLOW.md`)
- **Task ID:** `INFRA-007`
- **Task Title:** Multi-Agent Git Collaboration Workflow (`docs/GIT_WORKFLOW.md`)
- **Description:** Create `docs/GIT_WORKFLOW.md` specifying branch architecture, worktree isolation, commit conventions, checkpoint commits, peer review workflow, integration into `main`, branch synchronization, conflict resolution, and shared documentation governance.
- **Responsible Agent:** Google Antigravity
- **Priority:** Critical
- **Current Status:** Completed
- **Dependencies:** `DOC-001`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - `docs/GIT_WORKFLOW.md` covers all required sections (3.1–3.8) and aligns strictly with `AGENTS.md`.
- **Related Commit / PR:** `63b125c` (`docs: add multi-agent task tracker, handoff system, and git workflow`)
- **Last Updated Date:** 2026-11-17

#### `INFRA-008` — Agent-Specific Instruction Bridges and Worktree Baseline Synchronization
- **Task ID:** `INFRA-008`
- **Task Title:** Agent-Specific Instruction Bridges and Worktree Baseline Synchronization
- **Description:** Configure agent-specific instruction loading (where required for Claude Code and Antigravity) and synchronize approved Phase 0 documentation commits (`AGENTS.md`, `docs/TASK_TRACKER.md`, `docs/handoffs/`, `docs/GIT_WORKFLOW.md`) across `main`, `agent/codex`, `agent/claude`, and `agent/antigravity`.
- **Responsible Agent:** Google Antigravity
- **Priority:** High
- **Current Status:** Ready
- **Dependencies:** `DOC-001`, `INFRA-005`, `INFRA-006`, `INFRA-007`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - Approved Phase 0 commits are integrated into `main` with user authorization.
  - All three agent worktrees fast-forward cleanly to the synchronized `main` baseline.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

### Phase 1 — Project Research and Architecture

#### `ARCH-001` — Define ML Project Objectives and Problem Formulation
- **Task ID:** `ARCH-001`
- **Task Title:** Define ML Project Objectives and Problem Formulation
- **Description:** Formally specify the machine learning problem statement, input modalities, operational constraints, non-goals (no external hosted LLM inference APIs), and quantitative success criteria.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `INFRA-008`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Formal problem specification document is reviewed and approved by the user.
  - Scope boundaries, latency targets, and reproducibility constraints are unambiguously defined.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `ARCH-002` — Define Target Prediction Labels and Ambiguity/Abstention Schema
- **Task ID:** `ARCH-002`
- **Task Title:** Define Target Prediction Labels and Ambiguity/Abstention Schema
- **Description:** Specify the canonical target department taxonomy, integer class ID mapping, boundary resolution rules for overlapping complaints, and abstention/human-review fallback routing semantics.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `ARCH-001`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Canonical label map (`label_id <-> label_name`) and versioned schema are documented.
  - Rules for low-confidence abstention and out-of-distribution handling are explicitly specified.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `ARCH-003` — Determine Dataset Requirements and Schema Specification
- **Task ID:** `ARCH-003`
- **Task Title:** Determine Dataset Requirements and Schema Specification
- **Description:** Define the required dataset fields, data types, validation constraints, minimum sample counts per class, language coverage expectations, and file storage/manifest conventions.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `ARCH-002`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Formal dataset schema specification covering required/optional fields, checksum manifests, and split metadata is approved.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `ARCH-004` — Establish Modular ML System Architecture and Component Contracts
- **Task ID:** `ARCH-004`
- **Task Title:** Establish Modular ML System Architecture and Component Contracts
- **Description:** Design the Python package structure, configuration schema, and typed interfaces connecting data ingestion, preprocessing, model training, evaluation, serialization, and local inference.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `ARCH-002`, `ARCH-003`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Package layout and module interface contracts are documented and approved before implementation begins.
  - Clear separation between training-time fitting and inference-time transformation is enforced by design.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `ARCH-005` — Select Evaluation Methodology, Metrics, and Reproducibility Protocol
- **Task ID:** `ARCH-005`
- **Task Title:** Select Evaluation Methodology, Metrics, and Reproducibility Protocol
- **Description:** Define the mandatory evaluation protocol, primary and secondary metrics (Macro F1, Weighted F1, per-class Precision/Recall/F1, ECE, confusion matrix), random seed policy, and statistical comparison rules.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `ARCH-002`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Evaluation methodology specification is approved and establishes strict rules keeping the test split untouched until final model evaluation.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

### Phase 2 — Dataset Engineering

#### `DATA-001` — Dataset Sourcing Strategy and Provenance Manifest Design
- **Task ID:** `DATA-001`
- **Task Title:** Dataset Sourcing Strategy and Provenance Manifest Design
- **Description:** Define the approved dataset acquisition, synthetic fixture rules (for unit testing only vs. benchmark datasets), licensing/privacy compliance checks, and SHA-256 dataset manifest format.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `ARCH-003`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Dataset provenance, versioning, and manifest specification are documented and approved.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DATA-002` — Data Quality Validation and Schema Enforcement Pipeline
- **Task ID:** `DATA-002`
- **Task Title:** Data Quality Validation and Schema Enforcement Pipeline
- **Description:** Implement deterministic Python data validation utilities that verify record schemas, detect missing/corrupted fields, flag exact and near-duplicate records, and verify dataset file hashes.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `ARCH-003`, `ARCH-004`, `DATA-001`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Validation module rejects malformed records with descriptive domain exceptions.
  - Unit tests using small synthetic fixtures pass and are reviewed by Claude Code.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DATA-003` — Text Cleaning, Normalization, and PII Redaction Pipeline
- **Task ID:** `DATA-003`
- **Task Title:** Text Cleaning, Normalization, and PII Redaction Pipeline
- **Description:** Implement deterministic text normalization and privacy-aware PII masking (stripping email addresses, phone numbers, student IDs, and personal identifiers) prior to feature extraction or logging.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `DATA-002`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Text normalization and PII redaction functions are stateless, deterministic, and strongly typed.
  - Unit tests verify PII patterns are masked and no raw PII is emitted to logs.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DATA-004` — Labeling Strategy, Annotation Guidelines, and Class Mapping Utilities
- **Task ID:** `DATA-004`
- **Task Title:** Labeling Strategy, Annotation Guidelines, and Class Mapping Utilities
- **Description:** Establish the labeling guidelines, inter-annotator disagreement resolution protocol, and deterministic label encoder/decoder utilities conforming to `ARCH-002`.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Medium
- **Current Status:** Backlog
- **Dependencies:** `ARCH-002`, `DATA-001`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Labeling guidelines and bidirectional label mapping specification/tests are verified and approved.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DATA-005` — Leakage-Free Stratified Train/Validation/Test Split Generator and Privacy Audit
- **Task ID:** `DATA-005`
- **Task Title:** Leakage-Free Stratified Train/Validation/Test Split Generator and Privacy Audit
- **Description:** Implement seeded, stratified train/validation/test partitioning that groups related or near-duplicate records into the same split and exports verifiable split manifests.
- **Responsible Agent:** OpenAI Codex
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `DATA-002`, `DATA-003`, `DATA-004`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Split generator produces identical partitions given the same seed and dataset manifest.
  - Automated audit verifies zero record or group overlap across train, validation, and test splits.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

### Phase 3 — Baseline Machine Learning

#### `MODEL-001` — Reproducible Baseline Experiment Pipeline and Configuration Loader
- **Task ID:** `MODEL-001`
- **Task Title:** Reproducible Baseline Experiment Pipeline and Configuration Loader
- **Description:** Implement typed experiment configuration loading, deterministic global seeding, and structured experiment run metadata logging (hyperparameters, seed, dataset hash, Git commit hash).
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `ARCH-004`, `ARCH-005`, `DATA-005`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Experiment runner executes deterministically from structured configuration files and records full lineage metadata.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `MODEL-002` — Training-Only Text Preprocessing and TF-IDF Feature Extraction
- **Task ID:** `MODEL-002`
- **Task Title:** Training-Only Text Preprocessing and TF-IDF Feature Extraction
- **Description:** Implement vocabulary and TF-IDF feature extraction pipelines that fit strictly on the training split and transform validation and test splits without refitting.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-001`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Feature extractors enforce fit-on-train-only semantics and serialize vocabulary/IDF state deterministically.
  - Leakage tests confirm validation/test tokens never alter vocabulary or IDF weights.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `MODEL-003` — Classical Baseline Classifiers (Naive Bayes, Logistic Regression, Linear SVM)
- **Task ID:** `MODEL-003`
- **Task Title:** Classical Baseline Classifiers (Naive Bayes, Logistic Regression, Linear SVM)
- **Description:** Implement training and validation workflows for classical statistical baselines (Multinomial Naive Bayes, Logistic Regression, and Linear SVM) with configurable hyperparameters.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-002`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - All baseline classifiers train reproducibly, output class probability/score estimates, and save versioned artifacts.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `TEST-001` — Initial Baseline Evaluation Suite, Leakage Verification, and Statistical Audit
- **Task ID:** `TEST-001`
- **Task Title:** Initial Baseline Evaluation Suite, Leakage Verification, and Statistical Audit
- **Description:** Implement and verify the evaluation suite computing Macro F1, Weighted F1, per-class Precision/Recall/F1, and confusion matrices, and audit the baseline pipeline for data leakage and reproducibility.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `ARCH-005`, `MODEL-003`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Evaluation metrics match reference calculations on deterministic fixtures.
  - Formal review confirms zero train/validation/test leakage and full reproducibility.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DOC-002` — Baseline Evaluation Visualization and Confusion Matrix Reporting Tools
- **Task ID:** `DOC-002`
- **Task Title:** Baseline Evaluation Visualization and Confusion Matrix Reporting Tools
- **Description:** Build deterministic plotting and reporting utilities to render per-class metric comparisons and normalized/raw confusion matrices from verified baseline evaluation artifacts.
- **Responsible Agent:** Google Antigravity
- **Priority:** Medium
- **Current Status:** Backlog
- **Dependencies:** `TEST-001`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - Visualization scripts generate accurate charts strictly from actual evaluation output files without hardcoded numbers.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

### Phase 4 — Advanced Machine Learning

#### `MODEL-004` — Custom Neural Network Architecture and Training Pipeline Experiments
- **Task ID:** `MODEL-004`
- **Task Title:** Custom Neural Network Architecture and Training Pipeline Experiments
- **Description:** Implement scratch-initialized neural text classification architectures, seeded DataLoaders, training loops, early stopping on validation Macro F1, and checkpoint management as approved in Phase 1.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `TEST-001`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Neural training pipeline is deterministic under fixed seeds, logs epoch metrics, and compares fairly against Phase 3 baselines on the validation split.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `MODEL-005` — Confidence Calibration and Human-Review Abstention Thresholding
- **Task ID:** `MODEL-005`
- **Task Title:** Confidence Calibration and Human-Review Abstention Thresholding
- **Description:** Implement post-hoc probability calibration (e.g., temperature scaling / isotonic regression fitted on validation predictions) and configurable confidence thresholds for routing uncertain predictions to human review.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-003`, `MODEL-004`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Calibration module computes Expected Calibration Error (ECE) and supports explicit abstention/fallback routing below a configured threshold.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `TEST-002` — Structured Error Analysis, Calibration Verification, and Slice Audits
- **Task ID:** `TEST-002`
- **Task Title:** Structured Error Analysis, Calibration Verification, and Slice Audits
- **Description:** Audit neural and calibrated models across data slices (department class, text length buckets, confidence intervals, and high-loss misclassifications) and verify statistical validity.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-004`, `MODEL-005`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Slice evaluation and error analysis suite identifies failure modes and verifies calibration improvements without touching test labels for tuning.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `MODEL-006` — Optional Image and Multimodal Fusion Experiments (If Justified)
- **Task ID:** `MODEL-006`
- **Task Title:** Optional Image and Multimodal Fusion Experiments (If Justified)
- **Description:** If justified by Phase 1 dataset availability and Phase 3/4 findings, implement visual feature extraction and multimodal text-image fusion experiments with ablation comparisons.
- **Responsible Agent:** OpenAI Codex
- **Priority:** Low
- **Current Status:** Backlog
- **Dependencies:** `MODEL-004`, `TEST-002`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Executed only if image dataset requirements and user approval are satisfied; includes unimodal vs. multimodal ablation results.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DOC-003` — Interactive Calibration, Slice Error Analysis, and Experiment Comparison Views
- **Task ID:** `DOC-003`
- **Task Title:** Interactive Calibration, Slice Error Analysis, and Experiment Comparison Views
- **Description:** Create reliability diagrams (calibration curves), coverage-vs-accuracy trade-off plots for abstention thresholds, and slice error inspection views for developer diagnostics.
- **Responsible Agent:** Google Antigravity
- **Priority:** Medium
- **Current Status:** Backlog
- **Dependencies:** `MODEL-005`, `TEST-002`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - Diagnostic plots and views accurately render calibration curves, abstention trade-offs, and misclassified slice breakdowns from real experiment artifacts.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

### Phase 5 — Local Inference and Testing

#### `MODEL-007` — Versioned Model Serialization, Preprocessing Bundling, and Artifact Loader
- **Task ID:** `MODEL-007`
- **Task Title:** Versioned Model Serialization, Preprocessing Bundling, and Artifact Loader
- **Description:** Package trained model weights, fitted preprocessing artifacts, label maps, calibration parameters, and lineage metadata into a versioned, integrity-checked bundle with a deterministic loader.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-005`, `TEST-002`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Loaded model bundle reproduces identical predictions and calibrated probabilities to the training/evaluation environment.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `MODEL-008` — Local Prediction CLI and Self-Hosted Inference Service
- **Task ID:** `MODEL-008`
- **Task Title:** Local Prediction CLI and Self-Hosted Inference Service
- **Description:** Implement a local prediction CLI and self-hosted HTTP inference endpoint with strict input validation, PII-safe logging, calibrated confidence output, and human-review fallback flags.
- **Responsible Agent:** OpenAI Codex
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-007`
- **Assigned Git Branch:** `agent/codex`
- **Acceptance Criteria:**
  - Local CLI and API return predicted department, class ID, calibrated confidence, per-class probabilities, and abstention status with proper error handling.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `TEST-003` — End-to-End Unit, Integration, and Regression Test Suite
- **Task ID:** `TEST-003`
- **Task Title:** End-to-End Unit, Integration, and Regression Test Suite
- **Description:** Design and execute comprehensive automated unit, integration, and boundary tests covering data validation, preprocessing, model inference, abstention routing, and error handling.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `MODEL-008`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Full automated test suite runs offline using deterministic synthetic fixtures and passes with zero hidden failures.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `TEST-004` — Inference Latency, Determinism, and Reliability Verification
- **Task ID:** `TEST-004`
- **Task Title:** Inference Latency, Determinism, and Reliability Verification
- **Description:** Benchmark local inference latency, memory footprint, repeated-request determinism, and malformed-input resilience against Phase 1 acceptance criteria.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `MODEL-008`, `TEST-003`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Verification report records actual measured latency distribution, memory usage, and pass/fail results for boundary stress checks.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DOC-004` — Developer Inspection Interface and Browser-Based Verification Tooling
- **Task ID:** `DOC-004`
- **Task Title:** Developer Inspection Interface and Browser-Based Verification Tooling
- **Description:** When approved by the user, build a local developer inspection interface for testing complaint classification, inspecting confidence/calibration outputs, and verifying UI/API behavior in the browser.
- **Responsible Agent:** Google Antigravity
- **Priority:** Medium
- **Current Status:** Backlog
- **Dependencies:** `MODEL-008`, `TEST-003`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - Local inspection tool communicates cleanly with the local inference interface and passes browser verification checks.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

### Phase 6 — Documentation and Release Preparation

#### `TEST-005` — End-to-End Reproducibility and Clean-Room Verification Audit
- **Task ID:** `TEST-005`
- **Task Title:** End-to-End Reproducibility and Clean-Room Verification Audit
- **Description:** Verify that the complete pipeline—from dataset split verification through training, evaluation, and local inference—can be reproduced deterministically from documented commands and seeds.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `TEST-003`, `TEST-004`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Clean execution trace reproduces documented metrics within deterministic tolerances and passes security/privacy checks.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DOC-005` — Final Technical, Architecture, and Model Card Documentation
- **Task ID:** `DOC-005`
- **Task Title:** Final Technical, Architecture, and Model Card Documentation
- **Description:** Update project documentation, architecture diagrams, usage guides, and model evaluation documentation to reflect the actual verified implementation, metrics, and limitations.
- **Responsible Agent:** Google Antigravity
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `TEST-005`, `DOC-003`, `DOC-004`
- **Assigned Git Branch:** `agent/antigravity`
- **Acceptance Criteria:**
  - Documentation accurately reflects implemented modules, commands, and verified metrics with zero unverified claims.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `DOC-006` — Release Preparation, Dependency Audit, and Packaging Checklist
- **Task ID:** `DOC-006`
- **Task Title:** Release Preparation, Dependency Audit, and Packaging Checklist
- **Description:** Audit pinned dependencies, license compliance, `.gitignore` rules, artifact hygiene, and release readiness criteria.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** High
- **Current Status:** Backlog
- **Dependencies:** `TEST-005`, `DOC-005`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - Dependency manifest, security audit, and release checklist are verified with zero credential or PII leaks.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

#### `TEST-006` — Final Multi-Agent Quality Assurance Sign-Off
- **Task ID:** `TEST-006`
- **Task Title:** Final Multi-Agent Quality Assurance Sign-Off
- **Description:** Conduct final cross-agent review of code, tests, visualizations, and documentation before presenting the release candidate to the user for final approval.
- **Responsible Agent:** Anthropic Claude Code
- **Priority:** Critical
- **Current Status:** Backlog
- **Dependencies:** `DOC-006`
- **Assigned Git Branch:** `agent/claude`
- **Acceptance Criteria:**
  - All Phase 0–6 acceptance criteria are verified and signed off for user review.
- **Related Commit / PR:** None
- **Last Updated Date:** 2026-11-17

---

## 4. Tracker Maintenance Protocol

1. **When Starting a Task:** Verify all dependencies are `Completed`, update the task status from `Ready` to `In Progress` on the active branch, and record the current date.
2. **When Handing Off or Blocked:** Update status to `Blocked` or keep `In Progress` alongside a linked handoff record in `docs/handoffs/`.
3. **When Submitting for Review:** Record the relevant commit hash, set status to `In Review`, and notify the user / reviewing agent.
4. **Controlled Integration:** Updates to `docs/TASK_TRACKER.md` follow the controlled shared documentation process defined in `docs/GIT_WORKFLOW.md` to prevent conflicting concurrent edits across worktrees.
