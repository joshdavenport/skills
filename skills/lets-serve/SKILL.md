---
name: lets-serve
description: Write a cold-handoff-ready plan from a reached design understanding, run a fresh-context review of it, and hand off to a clean implementation session. Use after grill-me, lets-cook, brainstorming, simple, or any design conversation — or when user says "let's serve".
---

Externalize the design understanding reached in this session into a plan file that a sub-agent with ZERO session history can execute.

## Hard gate

Do NOT write code, scaffold files, or take implementation action in this session. The output of this skill is: a plan file, a cold-read review result, and handoff instructions. Nothing else.

## The bar

The plan must pass the **cold-reader test**: a sub-agent whose only inputs are this plan file and the repo can execute it without asking any questions. If a line in the plan only makes sense because you were in the design session, rewrite it.

## Step 1 — Write the plan

Path: `docs/plans/YYYY-MM-DD-<topic>.md` where `<topic>` is a short kebab-case slug.

Structure below. Scale each section to complexity — small work collapses sections to a sentence; large work expands them. Never skip Decisions or Open Questions.

### Context
What is being built/changed, why, what it replaces or extends. 1–3 paragraphs. Written for a reader with no session history. State conclusions directly — avoid "we decided" or "the approach we chose".

### Decisions
Every meaningful decision the design discussion resolved. For each:
- **Decision**: the choice made
- **Rejected**: alternatives considered
- **Why**: the reason (often a constraint, prior incident, or user preference)

This section is load-bearing. Without rejected alternatives + reasons, a fresh agent re-opens settled questions.

### Out of scope
Explicit bullet list of what this plan does NOT do. Kills scope drift.

### Steps
Ordered, numbered steps. Each step must be readable on its own without relying on earlier steps' context. For each:
- Files affected (exact paths)
- What changes (specific — symbols, line ranges where possible)
- Acceptance criterion (how to know the step is done)

State dependencies explicitly. If steps are independent, say so — it unlocks parallel sub-agent dispatch.

### Verification
How to confirm the whole plan worked. Tests to run, behaviours to check, manual QA if relevant.

### Open questions
Anything the design discussion did not resolve. Never hide these. A fresh agent must know not to invent answers. If there are none, write "None."

## Step 2 — Cold-read review

Dispatch a general-purpose sub-agent. The agent's context is the plan file + the repo. Its prompt:

> Read the plan at `<path>`. Your job: evaluate whether you could implement this plan without asking any questions. You have no session history — the plan is your only source of intent. List every ambiguity, implicit reference, vague verb, missing file path, unstated assumption, or underspecified acceptance criterion. Do not implement. Do not suggest improvements beyond listing gaps. Report under 300 words.

If the reviewer finds gaps, fix the plan and re-dispatch. Cap at 2 iterations — if still not clean, surface the remaining gaps to the user rather than polishing forever.

## Step 3 — Handoff

Once the plan passes cold-read review, output to the user (and only this):

1. The plan path
2. A suggestion to commit it if they want — do NOT commit automatically; plans are disposable
3. **Two handoff prompts** for a fresh session, labelled clearly. Tell the user to `/clear` or start a new session, then paste whichever fits this task:

   **A — Single commit at end (default for small/cohesive work):**
   > Read `<path>`. Execute steps 1–N. For each step: (1) state the change, (2) make it, (3) verify the step's acceptance criterion, (4) report what changed. Do not commit between steps; I'll review and commit the whole change at the end. If any step is ambiguous, stop and ask — do not guess.

   **B — Commit after each step (for larger work, refactors, or when bisect-friendly history matters):**
   > Read `<path>`. Execute steps 1–N. For each step: (1) state the change, (2) make it, (3) verify the step's acceptance criterion, (4) commit with a focused message referencing the step number and what it accomplished. Never amend prior commits. If any step is ambiguous, stop and ask — do not guess.

   One-line guidance: pick A if the plan is small or the steps don't form independently meaningful checkpoints; pick B if steps are substantial enough that each one is worth its own revertable commit.

4. A reminder to `/clear` or start a new session before running the chosen prompt

Then stop. This session is done.

## Principles

- **Cold-reader test is the bar.** If a line only makes sense because you were in the session, rewrite it.
- **Rejected alternatives are load-bearing.** Without them, fresh agents re-litigate.
- **Open questions must be visible.** Hidden uncertainty becomes confident wrongness downstream.
- **The plan is the persistence layer.** Once written and reviewed, the design session is disposable — and should be disposed of, via `/clear`.
