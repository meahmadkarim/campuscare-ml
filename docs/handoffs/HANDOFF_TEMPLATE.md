# Handoff Record: `HANDOFF-<TASK-ID>-<YYYY-MM-DD>`

> **Instructions for Use:** Copy this template to a new file in `docs/handoffs/` named `HANDOFF-<TASK-ID>-<YYYY-MM-DD>.md` (for example, `HANDOFF-MODEL-001-2026-11-17.md`). Fill out every section factually. Never modify `HANDOFF_TEMPLATE.md` directly to record a specific task handoff, never fabricate test outputs, and never include secrets, API keys, or raw private data.

---

## A. Task Identity

- **Task ID:** `<INFRA-xxx | ARCH-xxx | DATA-xxx | MODEL-xxx | TEST-xxx | DOC-xxx>`
- **Task Title:** `<Exact task title from docs/TASK_TRACKER.md>`
- **Date:** `<YYYY-MM-DD>`
- **Original Agent:** `<OpenAI Codex | Anthropic Claude Code | Google Antigravity>`
- **Receiving Agent:** `<OpenAI Codex | Anthropic Claude Code | Google Antigravity | User>`
- **Current Status:** `<Backlog | Ready | In Progress | Blocked | In Review | Completed>`
- **Handoff Reason:** `<Context/Token Limit | Usage/Rate Limit | Blocker Encountered | Ready for Peer Review | Role Transition>`

---

## B. Git Information

- **Repository:** `C:\Users\1000014573\My_Projects\CampusCare-ML`
- **Current Branch:** `<agent/codex | agent/claude | agent/antigravity | main>`
- **Worktree Directory:** `<.worktrees/codex | .worktrees/claude | .worktrees/antigravity | .>`
- **Latest Relevant Commit Hash:** `<7-to-40 character Git SHA, or "None (uncommitted)">`
- **Whether Work Is Committed:** `<Yes — fully committed | Partially committed | No — uncommitted in worktree>`
- **Whether There Are Uncommitted Changes:** `<No (working tree clean) | Yes — list exact modified/untracked files below>`

---

## C. Task Objective

`<Provide a clear, self-contained explanation of what this task is trying to accomplish, its scope boundaries, and any upstream dependencies or design specifications it relies upon. Write this so the receiving agent can understand the goal without access to previous chat history.>`

---

## D. Completed Work

- `<Concrete item 1 implemented, verified, or documented>`
- `<Concrete item 2 implemented, verified, or documented>`
- `<Key design or configuration decisions made and the rationale behind them>`

---

## E. Files Modified

| File Path (Relative to Repository Root) | Change Type (`Created` / `Modified` / `Deleted`) | Purpose of Change | Committed? (`Yes` / `No`) |
|---|---|---|---|
| `<path/to/file.py>` | `<Created | Modified | Deleted>` | `<Concise explanation of what changed and why>` | `<Yes | No>` |

---

## F. Tests and Verification

> **Strict Integrity Rule:** Report only commands that were actually executed and their real outputs. Never invent, extrapolate, or assume test passes.

### 1. Tests Performed
- `<Exact command 1 executed (e.g., pytest tests/unit/test_validation.py)>`
- `<Exact command 2 executed>`

### 2. Results
- `<Actual pass/fail summary and relevant output excerpt>`

### 3. Failed Checks
- `<List any failing tests, linter errors, or verification failures with exact error messages, or state "None">`

### 4. Tests Not Yet Executed
- `<List all required unit, integration, leakage, or browser checks that still need to be written or run before the task can be marked In Review / Completed>`

---

## G. Unfinished Work

1. `<Specific remaining implementation or documentation step 1, including target file/function names>`
2. `<Specific remaining test or verification step 2>`
3. `<Specific remaining documentation or tracker update step 3>`

---

## H. Known Issues

- **Bugs / Defects:** `<Describe any known bugs or failing edge cases, or state "None identified">`
- **Blockers:** `<Describe any missing specifications, upstream dependencies, or authorization requirements blocking progress, or state "None">`
- **Unresolved Decisions:** `<List any architectural, schema, or hyperparameter questions requiring Claude Code review or user approval>`
- **Technical Risks:** `<Highlight potential risks such as data leakage, memory constraints, edge-case inputs, or cross-worktree conflicts>`

---

## I. Next Recommended Actions

1. `<Immediate step 1 for the receiving agent (including exact branch/commit inspection command)>`
2. `<Step 2 to complete or review the remaining work>`
3. `<Step 3 to verify acceptance criteria and update docs/TASK_TRACKER.md>`

---

## J. Acceptance Criteria

Before this task (`<TASK-ID>`) can be transitioned to `Completed`, all of the following criteria must be satisfied and verified:

- [ ] `<Acceptance criterion 1 from docs/TASK_TRACKER.md>`
- [ ] `<Acceptance criterion 2 from docs/TASK_TRACKER.md>`
- [ ] `<All unit/verification checks pass with zero hidden failures>`
- [ ] `<Peer review completed by designated reviewer and integration approved by user>`
