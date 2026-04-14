---
description: Create a hybrid context/blueprint spec and git branch for a new feature
argument-hint: "Step number and feature name e.g. 2 registration"
allowed-tools: Read, Write, Glob, Bash(git:*)
---

You are a Senior Systems Architect for Spendly. Your goal is to initialize a feature development environment by creating a "Contract of Truth" (the Spec) and a clean git workspace. 

Follow these steps with precision.

## Step 1 — Integrity & Argument Parsing
1. Execute `git status --porcelain`. If output is NOT empty, STOP and notify user to commit/stash changes.
2. From `$ARGUMENTS`, extract:
   - `step_num`: Zero-pad to 2 digits (e.g., 3 -> "03").
   - `feat_title`: Title Case (e.g., "Login and Logout").
   - `feat_slug`: lowercase-kebab-case (max 40 chars).
   - `branch_name`: `feature/<feat_slug>`.

**Collision Logic:** If `git branch --list <branch_name>` exists, increment suffix (e.g., `feature/<feat_slug>-v2`).

## Step 2 — Workspace Synchronization
Execute the following sequence:
```bash
git checkout main
git pull origin main
git checkout -b <branch_name>
```

## Step 3 — Context Research
Analyze the codebase to prevent redundancy:
- **Roadmap:** Check `CLAUDE.md`. If this step is marked `[x]`, STOP and warn the user.
- **Database:** Review `database/db.py` or `schema.sql` to verify current table states.
- **Specs:** Check `.claude/specs/` to ensure logic doesn't conflict with existing features.

## Step 4 — Generate Hybrid Spec
Create `.claude/specs/<step_num>-<feat_slug>.md` with this exact two-section structure:

---
# Spec: <feat_title>

## 1. Context & User Story (Human Layer)
- **Overview:** 1-2 sentences on functional goals.
- **User Story:** "As a [role], I want to [action] so that [value]."
- **Depends on:** List previous Step IDs.

## 2. Technical Blueprint (Agent Layer)

### **Interface & Contract**
| Interface | Method | Path | Access | Action |
| :--- | :--- | :--- | :--- | :--- |
| [e.g. Route] | [POST] | [/path] | [Public/Auth] | [Logic description] |

### **Execution Plan**
- **Database:** List SQL changes or "Schema stable".
- **Modify:** List existing files + specific logic updates.
- **Create:** List new files + purpose.

### **Logic Scenarios (Gherkin-style)**
- **Scenario:** [Success/Failure case]
  - **When:** [Trigger condition]
  - **Then:** [Expected outcome]

### **Hard Constraints (The Guardrails)**
- Raw SQL only (No ORM); Parameterized queries (`?`).
- Passwords MUST be hashed with `werkzeug.security`.
- Use CSS variables (no hardcoded hex). Templates must `extend "base.html"`.
- Use `session.get('user_id')` to avoid KeyErrors in templates.

## 3. Definition of Done
- List 5-8 binary, testable requirements (e.g. "Duplicate email returns 400").
---

## Step 5 — Save & Finalize
Write the file to disk and ensure `CLAUDE.md` is updated (if applicable) to show the feature is now "In Progress."

## Step 6 — Handover Report
Print exactly:
```text
✅ Workspace Initialized
Branch:    <branch_name>
Spec:      .claude/specs/<step_num>-<feat_slug>.md
Status:    Sync'd with main. Roadmap verified.
```