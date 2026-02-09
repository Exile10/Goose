# Quick Git Reference for Jarida/Goose Workflow

## Your Current Setup

```
Your Machine: C:/Users/PC/Desktop/Goose
├── Remote: origin    → Your fork (Exile10/Goose)
├── Remote: jarida    → Organization repo (jarida-io/Goose)
└── Remote: upstream  → Block's repo (block/goose)

Current Branch: main
Status: 5 commits ahead of jarida/main
```

---

## Quick Commands

### See What's Ahead
```bash
git log --oneline jarida/main..main
# Shows: 5 commits with details
```

### See Detailed Changes
```bash
git diff jarida/main..main
# Shows: All code changes in those 5 commits
```

### See Changed Files Only
```bash
git diff --name-only jarida/main..main
```

### Push to Your Fork
```bash
git push origin main
```

### Push to Jarida Organization
```bash
git push jarida main
```

### Switch to Exile10 Branch
```bash
git checkout Exile10
```

### Update Exile10 from Latest Changes
```bash
git checkout Exile10
git merge main
```

---

## Typical Daily Workflow

### Morning: Sync with Latest
```bash
git fetch upstream
git fetch jarida
git fetch origin
git status
```

### During Day: Work on Your Branch
```bash
git checkout Exile10
# ... make changes ...
git add .
git commit -m "Your message"
git push origin Exile10
```

### Before Submitting PR: Test Compatibility
```bash
git fetch upstream
# Test merge without committing
git merge --no-commit --no-ff upstream/main
# If good, abort
git merge --abort
# If bad, resolve conflicts then commit
```

### Submit PR
```bash
# Through GitHub web interface
# https://github.com/Exile10/Goose/pulls
# Create PR to jarida-io/Goose:main
```

### After Approval: Sync
```bash
git push jarida Exile10
```

---

## Understanding the Remotes

```
upstream (block/goose)
    ↓ (sync to)
jarida (jarida-io/Goose)
    ↓ (sync to)
origin (your fork: Exile10/Goose)
    ↓ (branch from)
Exile10 (your working branch)
```

**Flow:**
1. Block releases changes → upstream
2. We sync to jarida (org repo)
3. You pull into your fork (origin)
4. You branch off (Exile10) for work
5. PR back through jarida to upstream

---

## Conflict Resolution Cheat Sheet

### If You See Merge Conflicts
```bash
# Stop the merge
git merge --abort

# Or resolve them
git status  # see conflicted files

# Edit the files to resolve conflicts
# Look for <<<<<<, ======, >>>>>>

# After fixing
git add .
git commit -m "Resolve merge conflicts"
```

### If Push Fails (Upstream Changes)
```bash
# Fetch latest
git fetch origin

# Rebase on latest
git rebase origin/main

# Or merge if you prefer
git merge origin/main

# Then push
git push origin main
```

---

## Checking Upstream Drift

### See if Upstream Has New Changes
```bash
git fetch upstream
git log upstream/main --oneline -10
```

### See Differences Between Repos
```bash
# Commits in upstream but not in jarida
git log jarida/main..upstream/main --oneline

# Commits in jarida but not in upstream
git log upstream/main..jarida/main --oneline

# How many ahead/behind
git rev-list --left-right --count upstream/main...jarida/main
```

---

## The 5 Commits Currently Ahead

```
a3ba12417 - tidy: clean up old benchmark and add gym (#7081)
d4865ae9f - fix: use command.process_group(0) for CLI providers (#7083)
5f183a63c - added build notify (#6891)
1371f5df4 - test(mcp): add image tool test and consolidate fixtures (#7019)
08b89ca66 - fix: remove Option from model listing return types (#7074)
```

**What to do:**
1. Review changes: `git log jarida/main..main --stat`
2. Test: `cargo test` or `npm test`
3. Push to origin: `git push origin main`
4. Push to jarida: `git push jarida main`
5. Create PR on GitHub

---

## Emergency Situations

### If You Accidentally Committed Wrong Things
```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Undo multiple commits
git reset --hard HEAD~3
```

### If You Pushed Wrong Commit to Origin
```bash
# Don't do git push --force unless absolutely necessary!
# Better: Create a new commit to revert
git revert COMMIT_HASH
git push origin main
```

### If You Need to Switch Branches
```bash
# Stash current work
git stash

# Switch branch
git checkout other-branch

# Come back and restore work
git checkout original-branch
git stash pop
```

---

## Key Files in This Repository

- **WORKFLOW_SETUP.md** - Complete workflow explanation
- **COMMITS_ANALYSIS.md** - Detailed analysis of the 5 commits
- **QUICK_REFERENCE.md** - This file (quick commands)
- **workflows.md** (in Downloads) - GitHub Actions workflows to implement

---

## GitHub Web URLs

| Action | URL |
|--------|-----|
| Your Fork | https://github.com/Exile10/Goose |
| Jarida Org | https://github.com/jarida-io/Goose |
| Upstream Block | https://github.com/block/goose |
| Your Fork PRs | https://github.com/Exile10/Goose/pulls |
| Create PR | https://github.com/Exile10/Goose/compare/jarida-io:main...main |

---

## Last Commands You Ran

```bash
# Your current status shows:
# - On branch main
# - 5 commits ahead of jarida/main
# - Working tree clean (no changes)
# - Remotes: jarida, origin, upstream all configured

# Next recommended action:
git log --oneline jarida/main..main
# Review these 5 commits and decide next steps
```

---

## Need Help?

- **Git Documentation**: `git help COMMAND`
- **Show your status**: `git status`
- **Show recent commits**: `git log --oneline -5`
- **Show all branches**: `git branch -a`
- **Show remotes**: `git remote -v`
