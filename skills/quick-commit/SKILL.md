---
name: quick-commit
description: Stage all changes, generate a Conventional Commits message from the diff, commit, and push to the current branch. Never pushes to main. Invoke whenever you are ready to commit work.
allowed-tools:
  - Bash
---

# /quick-commit

Stage, commit, and push in one shot. Message is generated from the diff - no manual writing needed.

## Step 1 - Read the current state

```bash
git status
git diff HEAD
git log --oneline -5
```

Read all three. Understand what changed before writing the message.

## Step 2 - Generate the commit message

Follow Conventional Commits format: `type: short description` in lowercase, no period.

Types:
- `feat` - new feature or capability
- `fix` - bug fix
- `chore` - maintenance, config, deps, cleanup
- `refactor` - restructuring without behavior change
- `style` - formatting, copy, visual changes
- `docs` - documentation only

One line. Under 72 characters. Describes WHAT changed and WHY, not HOW.

If the diff touches multiple concerns, use the most significant one as the type and summarize the group.

## Step 3 - Check the branch

```bash
git branch --show-current
```

If the current branch is `main` or `master`, stop and tell James. Never push directly to main.

## Step 4 - Stage and commit

```bash
git add -A
git commit -m "$(cat <<'EOF'
[generated message here]

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

## Step 5 - Push

```bash
git push origin $(git branch --show-current)
```

If the push fails because the remote branch doesn't exist yet:
```bash
git push --set-upstream origin $(git branch --show-current)
```

## Step 6 - Confirm

Report: what was committed, what branch, and the commit hash. One line.
