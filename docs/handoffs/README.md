# CampusCare-ML — Multi-Agent Handoff System and Protocol

This directory (`docs/handoffs/`) contains the formal handoff protocol and reusable template (`HANDOFF_TEMPLATE.md`) for transferring work safely between **OpenAI Codex** (`agent/codex`), **Anthropic Claude Code** (`agent/claude`), **Google Antigravity** (`agent/antigravity`), and the user.

---

## 1. Purpose and When to Trigger a Handoff

Because each agent operates in an isolated Git worktree and independent conversation session, context is not shared automatically across agents. A formal handoff record is required whenever an agent:

1. **Reaches a token or context window limit** before completing its assigned task.
2. **Reaches an API usage, quota, or rate limit** and must pause execution.
3. **Experiences a session interruption or connection failure** (handled via the Emergency Handoff procedure below).
4. **Cannot complete an assigned task** because remaining steps fall under another agent's primary role.
5. **Encounters a blocker** (missing specification, ambiguous requirement, failing upstream dependency, or required user authorization).
6. **Completes implementation and needs another agent to review, test, or continue its work.**

---

## 2. Critical Worktree and Git Isolation Realities

Every agent participating in a handoff must respect how Git worktrees operate in **CampusCare-ML**:

| Workspace / Agent | Assigned Branch | Assigned Worktree Directory |
|---|---|---|
| Main Repository | `main` | `.` (Repository root) |
| OpenAI Codex | `agent/codex` | `.worktrees/codex` |
| Anthropic Claude Code | `agent/claude` | `.worktrees/claude` |
| Google Antigravity | `agent/antigravity` | `.worktrees/antigravity` |

1. **Committed Objects Are Shared; Uncommitted Files Are Isolated:**
   - All four worktrees share the same underlying Git object database (`.git`). Once an agent creates a commit on its branch (e.g., `agent/codex`), that commit hash is immediately readable by any other agent via read-only Git commands (`git log agent/codex`, `git show <commit>`, `git diff`).
   - Conversely, **uncommitted changes** (modified or untracked files) exist only inside the specific physical worktree directory where they were edited and do **not** appear in any other worktree.
2. **Never Edit Another Agent's Worktree:**
   - A receiving agent must **never** `cd` into or edit files directly inside the original agent's worktree directory without explicit user instruction.
3. **No Simultaneous Checkout of the Same Branch:**
   - Git forbids checking out a branch (such as `agent/codex`) in a second worktree while it is already checked out in `.worktrees/codex`.
   - Receiving agents must continue work inside their own assigned worktree/branch by fetching/merging, cherry-picking, or branching from the original agent's committed checkpoint as described in Section 5.

---

## 3. Normal Handoff Workflow (10-Step Procedure)

When an active agent needs to transfer an unfinished task or submit completed work for cross-agent continuation/review, follow these 10 steps in order:

1. **Identify Unfinished Work:**
   - Inventory exactly what has been completed versus what remains against the task's acceptance criteria in `docs/TASK_TRACKER.md`.
2. **Check Git Status:**
   - Run `git status --short --branch` and `git diff` in the active worktree to inspect all modified, staged, and untracked files. Ensure no temporary scratch clutter or sensitive data is present.
3. **Run Available Relevant Tests:**
   - Execute all applicable unit tests, syntax checks, or verification commands and record the exact commands and real pass/fail outputs. Never fabricate test results.
4. **Commit Coherent Completed Changes (Where Authorized):**
   - If committing is authorized by the user, stage only the relevant task files and create a focused commit (or an explicitly labeled checkpoint commit, e.g., `chore(wip): checkpoint <TASK-ID> handoff state`) so the work is stored safely in the shared Git object database.
   - If committing is not yet authorized, leave the changes clean in the worktree and document the exact uncommitted file list in Section B and Section E of the handoff record.
5. **Create a Handoff Record:**
   - Copy `docs/handoffs/HANDOFF_TEMPLATE.md` to `docs/handoffs/HANDOFF-<TASK-ID>-<YYYY-MM-DD>.md` and complete all sections (`A` through `J`).
6. **Update the Central Task Tracker:**
   - Update the task's status (`In Progress`, `Blocked`, or `In Review`), related commit hash, and `Last Updated Date` in `docs/TASK_TRACKER.md`.
7. **Receiving Agent Reads Handoff and Documentation:**
   - The receiving agent begins in its own worktree (`agent/<receiving-agent>`), reads `AGENTS.md`, `docs/TASK_TRACKER.md`, and the specific `HANDOFF-<TASK-ID>-<YYYY-MM-DD>.md` file.
8. **Receiving Agent Continues via Approved Git Collaboration Process:**
   - Using the committed hash recorded in the handoff, the receiving agent inspects the changes (`git show <commit>` or `git diff main..<branch>`) and incorporates the committed work into its own branch (via authorized merge or cherry-pick) or creates review verification tests in its own branch without touching the original agent's worktree.
9. **Receiving Agent Verifies the Final Result:**
   - The receiving agent completes the remaining scope, runs the full verification suite, and confirms every acceptance criterion in Section J is satisfied.
10. **Submit Task for Review:**
    - The receiving agent records final verification evidence, updates `docs/TASK_TRACKER.md` to `In Review`, and presents the completed work to the reviewing agent / user for integration approval.

---

## 4. Emergency Handoff and Recovery Workflow (8-Step Procedure)

An **Emergency Handoff** occurs when the original agent terminates unexpectedly (token exhaustion, rate limit, crash, or lost session) **before** creating a handoff file or finishing its commit.

When the user assigns a replacement agent to recover and continue the interrupted task, the replacement agent must execute these 8 steps strictly:

1. **Inspect the Task Tracker:**
   - Read `docs/TASK_TRACKER.md` to identify the interrupted Task ID, its expected scope, assigned branch (`agent/<original-agent>`), dependencies, and acceptance criteria.
2. **Inspect Existing Git Commits:**
   - From the replacement agent's own worktree, run read-only Git inspection commands (`git log -n 10 --oneline agent/<original-agent>`) to see the latest commits recorded on the interrupted agent's branch.
3. **Inspect the Relevant Branch History and Diffs:**
   - Run `git diff main...agent/<original-agent>` and `git show <latest-commit>` to examine all committed code, tests, and documentation on the interrupted branch.
4. **Review the Most Recent Available Handoff:**
   - Check `docs/handoffs/` (both in the current branch and via `git ls-tree -r agent/<original-agent> docs/handoffs/`) for any prior handoff notes associated with the task.
5. **Identify What Is Safely Recoverable:**
   - Determine which parts of the task are already committed in Git history and whether those commits pass syntax and unit checks.
6. **Report Any Uncommitted Work That Cannot Be Accessed Safely:**
   - Check whether the interrupted agent's worktree has uncommitted changes using a **strictly read-only** status check (`git -C .worktrees/<original-agent> status --short`) if permitted by the environment, or report to the user if sandbox restrictions prevent inspecting the other worktree.
   - **Never** modify, stage, commit, or delete uncommitted files inside another agent's worktree unless the user explicitly authorizes a recovery commit or provides the diff.
7. **Avoid Destructive Cleanup:**
   - **Never** run `git reset --hard`, `git clean`, `git checkout --`, or `git worktree remove --force` on the interrupted agent's worktree. Uncommitted work in that directory must be preserved intact for user inspection.
8. **Continue Only Through an Approved Branch/Worktree Procedure:**
   - Once the user confirms how to proceed (for example, authorizing a checkpoint commit in the interrupted worktree or instructing the replacement agent to continue from the last valid commit on `agent/<original-agent>`), the replacement agent brings the approved commit(s) into its own assigned branch (`git merge <commit>` or `git cherry-pick <commit>`) and completes the task inside its own worktree.

---

## 5. Safe Cross-Worktree Continuation Patterns

Because two worktrees cannot check out the same branch simultaneously, use one of these three safe patterns when continuing committed work from another agent:

### Pattern 1: Fast-Forward or Merge from the Original Agent's Commit (Inside Receiving Agent's Branch)
When the original agent has committed its work (`<commit-sha>`) on `agent/codex` and `Claude Code` or `Antigravity` is authorized to continue on its own branch:
```powershell
# Executed inside the receiving agent's own worktree (e.g., .worktrees/claude on branch agent/claude)
git branch --show-current
git log -n 1 <commit-sha>
git merge --no-ff <commit-sha> -m "chore: integrate handoff commit <commit-sha> for <TASK-ID>"
```

### Pattern 2: Cherry-Picking Specific Task Commits
When the original agent's branch contains multiple commits and only specific task commits (`<commit-sha>`) should be transferred:
```powershell
# Executed inside the receiving agent's own worktree
git cherry-pick <commit-sha>
```

### Pattern 3: Read-Only Cross-Branch Review (For Peer Review Tasks)
When `Claude Code` is reviewing `Codex`'s completed work without needing to edit files:
```powershell
# Inspect diffs and files directly from Git objects without modifying any worktree
git diff main...agent/codex
git show agent/codex:path/to/file.py
```

---

## 6. Handoff File Naming Convention

- **Standard Format:** `HANDOFF-<TASK-ID>-<YYYY-MM-DD>.md`
  - Example: `HANDOFF-INFRA-001-2026-11-17.md`
  - Example: `HANDOFF-MODEL-002-2026-11-18.md`
- **Multiple Handoffs on the Same Day for the Same Task:** Append a sequence suffix (`-v2`, `-v3`), e.g., `HANDOFF-MODEL-002-2026-11-18-v2.md`.
- **Template Protection:** Always keep `HANDOFF_TEMPLATE.md` clean and unmodified as the blank master template.
- **No Fictional Records:** Create handoff files **only** when a real task handoff occurs.

---

## 7. Handoff Quality and Security Rules

Every handoff document created in `docs/handoffs/` must satisfy these quality standards:

1. **Factually Accurate:** Every claim regarding completed code, file paths, commit hashes, and test results must match the actual repository state.
2. **Concise Yet Self-Contained:** A receiving agent with zero prior conversation context must be able to read the handoff file and immediately understand what was done, why it was done, and what remains.
3. **Reproducible:** Include exact shell/test commands and configuration paths used to verify the work.
4. **Explicit About Unresolved Issues:** Never gloss over failing edge cases, skipped tests, or architectural ambiguities—list them prominently in Section F and Section H.
5. **Zero Secrets or PII:** Never include API keys, tokens, credentials, `.env` contents, or unredacted student complaint PII in any handoff record.
