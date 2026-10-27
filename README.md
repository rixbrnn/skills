# skills

Claude Code skills.

## Install

```bash
claude plugin add github.com/rixbrnn/skills
```

## Skills

### `cleanup-comments`

Removes noise comments from code, keeping only "why" comments per Uncle Bob's Clean Code rules.

**Usage:**

```
/cleanup-comments                  # current branch diff vs main
/cleanup-comments HEAD             # uncommitted working tree changes
/cleanup-comments main...feature   # explicit branch diff
/cleanup-comments -- src/auth/     # specific path
```

**What it removes:** narration/how comments, commented-out code, changelog references, redundant JSDoc.  
**What it keeps:** why comments, `TODO(TICKET-123)`, license headers.  
**Output:** `Removed N, rewritten M, across L lines in F files.`  
**Commits** automatically with `style: remove noise comments per Clean Code`.
