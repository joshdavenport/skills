---
name: explain-html
description: Build a rich, self-contained interactive HTML explainer page for any subject — a system, concept, process, incident, or dataset — saved as a dated HTML file. Provides the shared output mechanics (format, diagrams, optional quiz, validation, handoff) used by explain-diff and explain-decision, and can be invoked standalone when the user wants a visual HTML explanation of something that isn't a code diff or a decision.
---

# Explain HTML

Produce a single long-form, self-contained HTML page that teaches a reader about a subject. This skill serves two roles:

- **Standalone**: the user wants a visual HTML explainer for an arbitrary subject. Follow the standalone workflow below, then the mechanics.
- **As the output engine for another skill** (e.g. `explain-diff`, `explain-decision`): the invoking skill defines the workflow, narrative, and page sections. Skip the standalone workflow and apply only the mechanics, honoring any defaults the invoking skill specifies (such as its quiz recommendation).

## Standalone workflow

1. Identify the subject and the reader. Pin down what the page must let the reader do afterwards — understand a system, operate a process, evaluate a claim. If the subject is ambiguous, infer the most likely intent and state the assumption in the page.
2. Gather ground truth. Inspect real sources — code, documents, data, conversation context — rather than restating assertions from memory. Distinguish observed facts from reasonable interpretation.
3. Build a narrative before writing HTML: the motivating question, the smallest useful mental model, the concrete detail that realizes it, and the edge cases and consequences that matter.
4. Design a section structure that fits the subject (typically 3–5 sections moving from context to mechanism to consequences), with a table of contents linking to them. Then apply the mechanics below.

## Quiz opt-in

Before writing any HTML, ask the user whether they want an interactive quiz section appended to the page (use AskUserQuestion if available, otherwise ask in plain text). Keep it a single quick question. The invoking skill may state a default recommendation — present that as the recommended option but let the user decide. If the user declines, omit the quiz section entirely, including its table-of-contents entry.

## Output format

Write the output as one self-contained HTML file with inline CSS and JavaScript. Do not depend on external fonts, CDNs, images, JavaScript packages, or network access. Save it outside any code repository, preferably at `/tmp/YYYY-MM-DD-explanation-<slug>.html`, using the current date in `YYYY-MM-DD` format.

Include a clear title, a short summary, and a table of contents linking to the page's sections. Use smooth transitions, plain language, and precise systems-oriented prose. Explain jargon on first use. Use callouts for definitions, invariants, important edge cases, and practical consequences. Keep the page readable on phones with responsive CSS. Do not use top-level tabs; make it one continuous page.

## Diagrams and examples

Use a small, reusable set of HTML/CSS diagram patterns rather than ornamental graphics:

- flow diagrams for requests, data, or control flow;
- before/after or side-by-side comparison panels;
- labeled component or option cards for boundaries and alternatives;
- compact tables for mappings, criteria, invariants, and toy data.

Never use ASCII diagrams. Build diagrams with semantic HTML elements and CSS. Label arrows and include example values whenever the diagram describes data movement. Add accessible text or a caption so the explanation does not depend on visual inspection alone.

## Quiz quality rules (when the quiz is included)

Include exactly five medium-difficulty, interactive multiple-choice questions. Clicking an option must immediately show whether it is correct and explain why. Treat quiz design as part of the explanation, not decoration. Before emitting the page, inspect all five questions as a set.

- Randomize the option order independently for each question. Do not always place the correct answer first, second, or in any fixed position. A deterministic shuffle with a per-page seed is acceptable; the visible order must vary across questions.
- Balance correct-answer positions across the five questions as evenly as possible. Never let position, letter, punctuation, or a repeated pattern reveal the answer.
- Keep options comparable in length, grammar, specificity, and confidence. Do not make the correct option conspicuously longer, more qualified, or more technically precise than distractors. Shorten or enrich distractors as needed.
- Make every distractor plausible and tied to a real misunderstanding of the subject. Avoid joke answers, obviously impossible claims, "all/none of the above," and trivia that cannot be inferred from the page.
- Ask about behavior, causality, contracts, edge cases, trade-offs, or consequences. Avoid questions whose answer can be guessed from a single copied phrase.
- Keep the correct answer and explanation in the page's JavaScript data or DOM so the interaction works offline. Reveal feedback only after selection. Mark the selected option and explain both the right reasoning and, when useful, the misconception behind the distractors.
- Ensure the UI does not expose the answer through styling before selection, DOM labels, `title` attributes, source ordering, or accessibility text. Accessibility labels should describe the option, not its correctness.

## HTML and code-block constraints

- Escape user/code-derived text for HTML and JavaScript contexts. Preserve meaningful whitespace in code examples.
- Use `<pre><code>...</code></pre>` for code blocks. The CSS for `pre` must explicitly include `white-space: pre` or `white-space: pre-wrap`; verify every code block in the saved source before delivery.
- Keep JavaScript small, namespaced, and dependency-free. Use event listeners rather than inline handlers when convenient, and handle repeated interactive cards without relying on fragile global selectors.
- Include visible focus states and sufficient color contrast. Do not make correctness depend on color alone.
- Avoid claiming behavior or facts that the inspected sources do not support. Distinguish observed facts from reasonable interpretation.

## Validation

Validate the artifact before handing it off: confirm it exists, is a complete HTML document, contains no external asset dependencies, has working interactions (including the quiz, if included), and satisfies the code-block and quiz checks above. If practical, open it in a browser or use a local HTML inspection tool to catch layout or JavaScript errors.

## Final handoff

Return the exact absolute path to the generated HTML file as a clickable local-file link. Briefly state what was inspected and any assumptions or validation limitations. Do not place the deliverable inside a code repository unless the user explicitly requests that.
