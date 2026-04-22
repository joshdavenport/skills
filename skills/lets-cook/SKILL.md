---
name: lets-cook
description: Interview the user relentlessly about a plan or design until shared understanding, then externalize to a cold-handoff-ready plan via `lets-serve`. Use when user says "let's cook" or wants to go from idea to executable plan in one skill.
---

Interview the user relentlessly about every aspect of this plan until we reach shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time.

If a question can be answered by exploring the codebase, explore the codebase instead.

## Terminal state

When the interview has resolved, STOP asking questions and invoke the `lets-serve` skill. Do NOT write code. Do NOT implement. `lets-serve` is the only next step.

The interview is resolved when:
- The latest question returned no new branches
- The user signals alignment ("that's everything", "I think we're done", similar)
- You have run out of *meaningful* questions — not out of detail to chase

When in doubt, ask: *"I think we've resolved everything I can see — ready to serve?"* If yes, invoke `lets-serve` immediately.

## Hard gate

No code, no scaffolding, no file writes other than those `lets-serve` produces. The only terminal state is `lets-serve`.
