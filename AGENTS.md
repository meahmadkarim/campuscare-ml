# AGENTS.md — Shared Multi-Agent Development Instructions for CampusCare-ML

This document is the central, authoritative source of shared development instructions, engineering standards, and operational boundaries for all AI coding agents working on **CampusCare-ML**.

> **Instruction Loading Note:** OpenAI Codex natively reads `AGENTS.md` from the repository root. Do **not** assume that Anthropic Claude Code or Google Antigravity automatically load `AGENTS.md` without dedicated configuration (such as `CLAUDE.md`, `.gemini/`, or workspace-level rule bridges). Agent-specific instruction files and synchronization across worktrees are configured separately.

---

## 1. Project Identity

- **Project Name:** CampusCare-ML
- **Repository Structure:** Standalone, independent Git repository (`C:\Users\1000014573\My_Projects\CampusCare-ML`) dedicated strictly to Python-based machine learning engineering, experimentation, and local inference tooling.
- **Core Principles:**
  - **Scientific Reproducibility:** Every dataset split, preprocessing transformation, hyperparameter configuration, random seed, and evaluation metric must be deterministic, versioned, and reproducible.
  - **Engineering Maintainability:** Code must be modular, strongly typed, well-tested, and decoupled from external application logic.
- **Current Development Status:** **Early Development (Infrastructure & Specification Stage).** No machine learning pipelines, trained models, datasets, or inference endpoints have been implemented yet. Existing files under `docs/` represent preliminary planning notes only.
- **Strict Truthfulness Requirement:** Agents must **never** present planned, proposed, or stubbed functionality as completed work, and must never report fabricated metrics, synthetic test passes, or unverified claims.

---

## 2. Multi-Agent Architecture

Development is divided among three specialized AI coding agents operating in isolated Git worktrees branched from `main`. While agents collaborate across the system lifecycle, task ownership must remain unambiguous.

| Agent | Assigned Branch | Assigned Worktree | Primary Role |
|---|---|---|---|
| **OpenAI Codex** | `agent/codex` | `.worktrees/codex` | ML Implementation Engineer |
| **Anthropic Claude Code** | `agent/claude` | `.worktrees/claude` | Architecture and Quality Engineer |
| **Google Antigravity** | `agent/antigravity` | `.worktrees/antigravity` | Visualization and Developer Experience Engineer |

### 2.1 Codex — ML Implementation Engineer

Primary ownership:
- Core Python package and module implementation
- Data ingestion, validation, and preprocessing pipelines
- Feature engineering and representation extraction
- Classical ML and neural network training pipelines
- Model evaluation metric implementation
- Model serialization, artifact loading, and checkpoint management
- Local inference service and CLI implementation
- Runtime debugging, profiling, and performance optimization

### 2.2 Claude Code — Architecture and Quality Engineer

Primary ownership:
- Software and ML system architecture decisions and reviews
- Code quality, maintainability, and static analysis audits
- Unit, integration, and regression test suite design and verification
- ML evaluation methodology, experimental rigor, and baseline validation
- Statistical correctness, calibration analysis, and data leakage audits
- End-to-end reproducibility verification
- Security, dependency safety, and data privacy reviews
- Cross-component contract verification and integration planning

### 2.3 Google Antigravity — Visualization and Developer Experience Engineer

Primary ownership:
- Developer-facing tools, CLIs, and inspection interfaces
- Experiment tracking plots, confusion matrices, calibration curves, and error-analysis visualizations
- Evaluation dashboards and interactive diagnostic views (when explicitly approved)
- UI/UX design for human-in-the-loop review or model inspection tools
- Browser-based verification and interface testing
- Developer experience (DX) automation and multi-root VS Code workspace enhancements

---

## 3. Git and Worktree Rules

1. **Integration Baseline (`main`):**
   - `main` (checked out in the repository root `.`) is the stable integration branch.
   - Direct feature coding on `main` is prohibited unless the user explicitly instructs an infrastructure update on `main`.
2. **Worktree Isolation:**
   - Each agent must operate exclusively within its assigned worktree and branch (`agent/codex` in `.worktrees/codex`, `agent/claude` in `.worktrees/claude`, `agent/antigravity` in `.worktrees/antigravity`).
   - Before modifying any file, an agent must verify its working directory (`pwd` / `Get-Location`) and active Git branch (`git branch --show-current`).
   - Agents must **never** edit, delete, or stage files inside another agent's worktree or the root `main` checkout without explicit user authorization.
3. **Branch Synchronization Awareness:**
   - Commits made on `main` or on one agent branch do **not** automatically propagate to other existing worktrees.
   - Agents must not rebase, merge, or pull across branches unless explicitly instructed by the user.
4. **Non-Destructive Git Operations:**
   - Never run `git reset --hard`, `git checkout -- <file>`, `git restore`, `git clean`, `git branch -D`, `git worktree remove --force`, or history-rewriting commands (`git commit --amend`, interactive rebase) without explicit user approval.
   - Never discard uncommitted user or agent work.
5. **Commit Hygiene and Authorization:**
   - Always inspect `git status` and `git diff` prior to staging or committing.
   - Stage only files directly relevant to the assigned task.
   - Write concise, conventional commit messages (e.g., `feat:`, `fix:`, `test:`, `docs:`, `chore:`) when authorized to commit.
   - **Never** automatically merge into `main`.
   - **Never** push (`git push` or `git push --force`) to remote repositories without explicit user authorization.
6. **Sensitive and Large Artifact Protection:**
   - Never commit `.env` files, API keys, tokens, credentials, personally identifiable information (PII), raw unapproved datasets, or large binary model checkpoints without explicit user approval.

---

## 4. Python Coding Standards

All Python code written for CampusCare-ML must adhere to the following standards:

1. **Style and Formatting (PEP 8):**
   - Follow PEP 8 conventions consistently: 4-space indentation, UTF-8 encoding, `\n` line endings, and clear import ordering (standard library, third-party packages, local modules).
2. **Naming Conventions:**
   - `snake_case` for modules, packages, functions, methods, and variables.
   - `PascalCase` for classes, exceptions, and typed schemas/dataclasses.
   - `UPPER_SNAKE_CASE` for module-level constants.
3. **Type Hints:**
   - Provide explicit Python type annotations for all function signatures (parameters and return values), class attributes, and public interfaces using `typing` / standard collection generics (Python 3.10+ syntax).
4. **Docstrings:**
   - Include clear docstrings on all public modules, classes, and functions explaining purpose, arguments, return values, raised exceptions, and shape/type expectations for tensors or arrays.
5. **Modularity and Single Responsibility:**
   - Keep functions focused and composable. Separate data I/O, feature transformations, model definitions, training loops, evaluation metrics, and visualization logic into distinct modules.
   - Avoid duplicated logic (DRY); extract shared utilities into dedicated common modules once interfaces are agreed upon.
6. **Exception Handling:**
   - Raise specific, descriptive exceptions (or custom domain exceptions) with actionable error messages.
   - Never use bare `except:` blocks or silently swallow errors.
7. **Logging:**
   - Use Python's standard `logging` module with structured, informative messages and appropriate severity levels (`DEBUG`, `INFO`, `WARNING`, `ERROR`) rather than ad-hoc `print()` statements in library code.
8. **Configuration Management:**
   - Externalize hyperparameters, file paths, thresholds, and runtime settings into typed configuration objects or structured config files (e.g., YAML/TOML/JSON/dataclasses) rather than hardcoding magic numbers inside logic.
9. **Dependency Management:**
   - Keep dependencies minimal, explicit, and pinned/documented in the project's approved dependency manifest once initialized.
   - Do not install new packages, system binaries, or virtual environments without user approval.
10. **Unit Testing:**
    - Design functions to be deterministic and testable in isolation using small synthetic fixtures without requiring full datasets or GPU hardware.

---

## 5. Machine Learning Engineering Standards

1. **No Premature Architectural Lock-In:**
   - Do not hardcode or prescribe a final production model architecture before the formal project specification and baseline experiments are approved.
2. **Deterministic Reproducibility:**
   - Explicitly seed all sources of randomness (`random`, `numpy`, `torch`, dataloader workers, and split generators) and record the seed in experiment metadata.
3. **Dataset Version Awareness:**
   - Track dataset schema versions, class label mappings, and file hashes/manifests so every training or evaluation run can be traced to an exact dataset state.
4. **Strict Split Isolation & Data Leakage Prevention:**
   - Partition data strictly into **train**, **validation**, and **test** sets before fitting any stateful transformation.
   - Fit tokenizers, vocabularies, TF-IDF vectorizers, scalers, imputers, and class-weight calculators **strictly on the training split only**, then apply them to validation and test splits without refitting.
   - Verify that duplicate or near-duplicate records (or records from the same complaint thread) do not span across splits.
5. **Evaluation Rigor:**
   - Report comprehensive metrics appropriate for potentially imbalanced multiclass classification, including **macro F1**, **weighted F1**, **per-class precision/recall/F1**, and **confusion matrices**.
   - Keep the held-out test set untouched during hyperparameter tuning and architecture selection; use validation splits or cross-validation for model selection.
6. **Confidence Calibration and Uncertainty Handling:**
   - Evaluate probability calibration (e.g., Expected Calibration Error / reliability diagrams) and support explicit abstention/fallback thresholds for routing low-confidence or out-of-distribution inputs to human review.
7. **Error Analysis:**
   - Provide structured slice and error analysis (e.g., performance by class, text length, confidence bucket, and high-loss misclassifications) rather than single aggregate numbers.
8. **Model & Experiment Lineage:**
   - Version serialized model artifacts alongside their preprocessing artifacts, label maps, hyperparameters, git commit hash, and evaluation summary.
9. **Privacy-Aware Data Processing:**
   - Ensure preprocessing and logging pipelines strip or mask personally identifiable information (PII) such as student names, IDs, email addresses, phone numbers, or sensitive personal details. Never log raw private records in plain text.
10. **Scientific Integrity (Zero Fabricated Results):**
    - Never invent, extrapolate, or hardcode placeholder accuracy/F1 numbers as real experiment results. Every reported metric must be produced by an actual, reproducible execution trace.

---

## 6. Agent Collaboration Rules

Every agent must follow this 10-step operational workflow on every task:

1. **Read Shared Instructions:** Read `AGENTS.md` and any approved project specifications before planning or writing code.
2. **Identify Assigned Task:** Confirm the exact task scope, acceptance criteria, and owning agent role.
3. **Verify Branch and Worktree:** Confirm execution is inside the agent's own worktree (`.worktrees/<agent>`) on its assigned branch (`agent/<agent>`).
4. **Inspect Dependencies and Interfaces:** Check upstream modules, data contracts, and shared schemas that the task depends on.
5. **Implement Assigned Scope Only:** Write only the code, tests, or configurations within the assigned boundary; do not perform unrequested refactoring or scope creep.
6. **Verify and Test Work:** Run applicable unit checks, syntax checks, or verification commands and inspect actual outputs.
7. **Document Key Decisions:** Record assumptions, edge cases, and configuration choices clearly in code comments, docstrings, or approved task notes.
8. **Report Changes and Limitations:** Accurately list every created or modified file, verification command executed, and any known constraints or edge cases.
9. **Prepare Work for Review:** Ensure `git status` and `git diff` are clean, free of temporary scratch clutter, and ready for cross-agent or user inspection.
10. **Stop When Blocked:** If requirements are ambiguous, an interface contract is missing, or an operation requires elevated permission, **stop immediately** and ask the user rather than guessing.

**Interface Stability Rule:** No agent may unilaterally change a shared function signature, data schema, file format, or API contract that another agent depends on without prior coordination and user approval.

---

## 7. Code Review Rules

Before any change on an agent branch (`agent/codex`, `agent/claude`, or `agent/antigravity`) is integrated into `main`:

1. **Mandatory Pre-Integration Review:** All changes must be reviewed (typically by Claude Code or the user) against acceptance criteria before merging into `main`.
2. **No Self-Approval:** An agent must never mark its own implementation as approved for `main` integration or merge its own branch into `main`.
3. **Testing Expectations:** New or modified functionality must include corresponding automated tests (or reproducible verification checks) covering normal execution, boundary conditions, and error handling.
4. **Transparent Failure Reporting:** Any failing test, skipped check, warning, or environment limitation must be reported explicitly with exact error output. Hiding or bypassing failing tests is strictly forbidden.
5. **Dependency and Security Audit:** Reviews must verify that no unauthorized dependencies, hardcoded secrets, insecure deserialization patterns, or PII exposures were introduced.
6. **Architectural and Statistical Consistency:** Reviews must confirm adherence to modular boundaries, absence of train/test data leakage, and deterministic reproducibility.
7. **Acceptance Criteria Verification:** Every item in the task's acceptance criteria must be checked against tangible evidence in the diff and test output.

---

## 8. Handoff Principles

When an agent completes a task stage or transfers work to another agent (or to the user for coordination), it must provide a structured handoff summary containing all nine of the following elements:

1. **Task Objective:** A concise statement of the assigned goal and scope.
2. **Branch and Worktree:** The active Git branch and worktree directory path used.
3. **Relevant Commit:** The current HEAD commit hash (and any new local commit hashes if committing was authorized).
4. **Files Changed:** Explicit list of all files created, modified, or deleted.
5. **Completed Work:** Concrete summary of what was implemented or verified.
6. **Tests and Results:** Exact verification or test commands executed and their pass/fail outputs.
7. **Known Problems:** Any bugs, failing edge cases, warnings, or environmental blocks encountered.
8. **Remaining Work:** Specific items still left to implement, test, or verify.
9. **Recommended Next Action:** Which agent (or user decision) should act next and what the immediate next step is.

*(Note: Formal handoff templates and task tracking files will be established in a dedicated subsequent step.)*

---

## 9. Conflict Resolution

When instructions, design proposals, or agent recommendations appear to conflict, resolve them strictly according to the following priority order (from highest to lowest authority):

1. **Explicit User Instructions** (highest authority)
2. **Approved Project Specifications**
3. **Shared Development Policies (`AGENTS.md`)**
4. **Approved Architecture Decisions**
5. **Assigned Task Requirements**
6. **Individual Agent Recommendations** (lowest authority)

**Escalation Rule:** If a conflict cannot be unambiguously resolved using the hierarchy above, or if two high-priority requirements appear contradictory, the agent must **pause execution**, document the conflict clearly, and escalate the decision to the user.

---

## 10. Safety and Authorization

Across all worktrees and sessions, agents must **never**:

1. **Delete user files** without explicit permission.
2. **Rewrite Git history** or perform destructive Git operations (`reset --hard`, `push --force`, `clean -fd`, forced branch/worktree deletion).
3. **Expose credentials**, API keys, tokens, or local environment secrets in code, logs, commits, or chat output.
4. **Upload private datasets** or transmit sensitive data to external services.
5. **Execute destructive system commands** or modify files outside `C:\Users\1000014573\My_Projects\CampusCare-ML`.
6. **Install paid services**, cloud resources, or unauthorized software packages without explicit user authorization.
7. **Change another agent's configuration**, provider settings, authentication, or worktree files.
8. **Claim unverified work is completed** or fabricate test/evaluation outputs.
9. **Make major architectural changes** or alter shared interfaces without prior user approval.
10. **Perform unrelated modifications** outside the scope of the assigned task.
