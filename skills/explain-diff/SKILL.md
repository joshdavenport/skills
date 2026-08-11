---
name: explain-diff
description: Create a rich, self-contained interactive HTML explanation of a code change, diff, branch, or pull request. Use when the user wants to understand the background, intuition, implementation, data flow, diagrams, or quiz-based reinforcement for a software change, with the result saved as a dated HTML file outside the repository.
---

# Explain Diff

Produce a single long-form HTML page that teaches a reader how a specified code change works. Investigate the surrounding system before explaining the diff: the page should make sense to a beginner while still giving an experienced engineer a concise path to the changed behavior.

## Workflow

1. Identify the change and its scope. Use the current checkout, diff, branch, PR metadata, or user-supplied files as the source of truth. If the target is ambiguous, infer the most likely change from the available context and state the assumption in the page.
2. Explore relevant surrounding code, tests, configuration, callers, data models, and documentation. Trace the old and new paths far enough to explain behavior, not merely file-by-file edits. Prefer checked-in examples and tests over speculation.
3. Build a narrative before writing HTML:
   - what problem or constraint motivated the change;
   - how the old system behaved;
   - the smallest useful mental model of the new behavior;
   - how the implementation realizes that model;
   - edge cases, trade-offs, and observable consequences.
4. Invoke the `explain-html` skill (via the Skill tool) and follow its mechanics sections for everything else: the quiz opt-in prompt, output format, diagram patterns, quiz quality rules, HTML constraints, validation, and handoff. Skip its standalone workflow — this skill supplies the workflow and page sections. For the quiz opt-in, recommend including it — quiz reinforcement is usually valuable for understanding a code change.

## Page sections

The table of contents links to these sections in this order:

1. **Background** — Explain only the system needed for the change. Start with an optional beginner-friendly mental model, then narrow to the exact components, contracts, and prior behavior involved.
2. **Intuition** — Explain the core idea before implementation detail. Use small concrete toy inputs and outputs. Show the old and new behavior when comparison makes the change clearer.
3. **Code** — Walk through the changes in conceptual groups, ordered by execution or dependency flow rather than arbitrary file order. Include precise file and line references when available, but do not dump the whole diff.
4. **Quiz** (if the user opted in) — Five interactive multiple-choice questions about behavior, causality, and code paths, per the shared quiz quality rules.

Diagram emphasis for diffs: flow diagrams for the changed request/data/control paths, and before/after panels contrasting old and new behavior.
