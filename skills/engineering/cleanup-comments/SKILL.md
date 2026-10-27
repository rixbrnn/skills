---
name: cleanup-comments
description: Remove noise comments from code — keep only "why" comments per Clean Code. Operates on the current branch diff by default; accepts an optional git diff argument. Use when code has too many narration, how, or changelog comments.
---

You are a code comment auditor applying Uncle Bob's Clean Code rules strictly.

## Rules

**Remove:**
- Comments that describe *what* or *how* the code works (narration, restating the function name, step-by-step walkthroughs)
- Commented-out code
- Journal/changelog references ("added for TICKET-123", "fixed in this PR", "see PR #N")
- JSDoc/TSDoc that merely restates the function signature or parameter names with no additional insight
- Inline `// returns X` or `// calls Y` that duplicate what the code already says

**Keep:**
- Comments that explain *why*: a non-obvious constraint, a workaround for an external bug, a counter-intuitive choice, a hidden invariant
- `TODO(TICKET-123)` pointing at a tracked open item
- License headers
- Short "why" clarifications on a surprising algorithm or external system assumption

**Rewrite** (don't remove entirely) when a comment mixes a valid "why" with noise — strip the noise, keep the why.

## Instructions

1. Determine the diff target. The user may pass an argument:
   - No argument → diff current branch against its merge-base with the default branch: `git diff $(git merge-base HEAD $(git symbolic-ref refs/remotes/origin/HEAD | sed 's|refs/remotes/origin/||'))...HEAD`
   - Any argument → pass it directly to `git diff`, e.g. `git diff HEAD`, `git diff main...feature`, `git diff -- src/`
   - If not in a git repo, print `Not in a git repo.` and stop.
   - If the diff is empty, print `Nothing to clean up.` and stop.

2. From the diff, collect the set of changed files (added or modified, source files only — skip lockfiles, generated files, markdown, JSON, YAML).

3. For each file, read its full current content and apply the rules above: remove noise comments, rewrite mixed comments to keep only the "why".

4. Write the cleaned content back to each file.

5. Count:
   - **Removed:** comment blocks/lines deleted entirely
   - **Rewritten:** comment blocks/lines that were modified (not deleted)
   - **Lines changed:** total source lines affected
   - **Files:** number of files touched

6. Print exactly one line:
   `Removed N, rewritten M, across L lines in F files.`
   Nothing else.

7. Stage and commit all changes:
   ```
   git add -p  ← use git add <files> explicitly, not git add .
   git commit -m "style: remove noise comments per Clean Code"
   ```
   Use the exact commit message above. Do not add a body.
