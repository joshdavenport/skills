---
name: asana-task
description: Work on a provided Asana task
---

## Task Understanding

Load the asana task given (which may be an ID or a URL) using the Asana MCP. Understand the task, reading: 
title, description, metadata, any attachments and any comments.

## Git Workflow

By default, simply do the work on the currently checkout branch. However, if the user requests to use a fix branch
in their prompt (e.g. "with branch", "do work on a branch", "use a branch" "on fix branch" etc) then create a
`fix/` prefixed branch using the task ID. For example, if the Asana task ID is 1234 then the branch would be
`fix/asana-1234`.

## Working on the Task

Work on resolving the task and revert to the user if there are ambiguities after analysing the task and the 
codebase. If there are many unknowns, grill the user on the resolution.

## Task Completion

Once implementation is complete, confirm the user is happy and if they confirm then:

1. Commit
2. Move the task to the "Ready to Push" section if there is one in the board
3. Set the "Progress" custom field to "Ready to Push"

If the fix work was executed on `fix/` prefixed branch, ask the user if they would like you to merge that 
change back into a working branch such as `main`, `develop`, `master`. Each project may have a different workflow.
The user may have specified what they would like in this regard in their orignial prompt.
