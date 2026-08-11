---
name: explain-decision
description: Create a rich, self-contained interactive HTML explanation of a proposed decision, design choice, or trade-off under discussion. Use when the user wants a visual walkthrough of a decision's context, the options considered, evaluation criteria, trade-offs, and recommendation — for example mid-way through a grilling or planning session — with the result saved as a dated HTML file outside the repository.
---

# Explain Decision

Produce a single long-form HTML page that teaches a reader what a decision is really about: the question at stake, the options on the table, how they trade off, and what choosing each one would mean. The page should let someone who wasn't in the conversation evaluate the decision cold, while giving the participants a shared visual reference.

## Workflow

1. Identify the decision and its scope. The source of truth is usually conversational: the current discussion (e.g. a grilling session), a plan document, an ADR draft, or user-supplied notes. Pin down the exact question being decided — if it is really several entangled decisions, say so and either pick the load-bearing one or structure the page around the dependency between them. State any assumptions in the page.
2. Gather the ground truth behind each option. Where options make claims about code, architecture, cost, or external systems, inspect the relevant sources rather than restating assertions from the conversation. Distinguish what has been verified from what is believed. Capture options that were raised and rejected — the rejection reasoning is often the most valuable content.
3. Build a narrative before writing HTML:
   - what forces or constraints make this a decision at all (why "do nothing" isn't free);
   - the criteria that matter, stated explicitly, including who or what each criterion protects;
   - each option's honest best case and worst case, not a strawman;
   - where the options genuinely differ versus where they converge;
   - the recommendation (or the open state, if undecided) and its consequences, including what becomes harder.
4. Invoke the `explain-html` skill (via the Skill tool) and follow its mechanics sections for everything else: the quiz opt-in prompt, output format, diagram patterns, quiz quality rules, HTML constraints, validation, and handoff. Skip its standalone workflow — this skill supplies the workflow and page sections. For the quiz opt-in, recommend skipping it — decision pages are usually a discussion aid rather than study material — but include it if the user wants to stress-test their grasp of the trade-offs.

## Page sections

The table of contents links to these sections in this order:

1. **Context** — The situation and constraints that force a choice. Only the background needed to understand why this decision exists and what it touches.
2. **The Question** — The decision stated as a single sharp question, plus the explicit criteria it will be judged by.
3. **Options** — Each option presented at its strongest: what it is, how it would work, a concrete toy scenario showing it in action, best case and worst case. Include rejected options with the reason for rejection.
4. **Trade-offs** — Direct comparison across the stated criteria. Highlight the genuine points of divergence; collapse the dimensions where options are equivalent rather than padding the comparison.
5. **Recommendation & Consequences** — The recommended option (or the honest open state), what committing to it makes easy, what it makes hard or forecloses, and what signal would indicate the decision should be revisited.
6. **Quiz** (if the user opted in) — Five interactive multiple-choice questions about consequences and trade-offs — "which option dominates under scenario X" — per the shared quiz quality rules.

Diagram emphasis for decisions: labeled option cards, criteria comparison tables, side-by-side consequence panels, and decision trees when options branch on a condition. Use concrete example values in scenarios, not abstract placeholders.
