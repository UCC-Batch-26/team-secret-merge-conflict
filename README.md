# Git Merge Conflicts

## Mental Model

- A conflict happens when two commits changed the same lines or Git cannot auto-merge file moves or deletes.
- Conflicts are normal. They mean collaboration is real.
- Your job is to decide what the final content should be, then tell Git "this file is resolved."

### Useful Commands

Make sure you have the latest branches

```sh
git checkout main
git pull
```

Merge `main` branch into your feature

```sh
git checkout my-feature
git merge main
```

At this point, one of two things happens:

✅ If there are no conflicts, Git will automatically merge. Just push:

```sh
git push origin my-feature
```

🚨 If there are conflicts, Git will pause and show files in conflict.

You'll need to open the conflicted files, fix them, then run:

```sh
git add <filename>
git commit
git push origin my-feature
```

## Resolve with an Editor or Tool

- Open the conflicted file. Editors show buttons like `Accept Current`, `Accept Incoming`, `Accept Both`, `Compare`.
- Choose the correct resolution and save.
- Then run:

```sh
git add <file>
git commit
```

## Common resolution strategies from the CLI

Keep both changes in order you choose.

Manually edit the included both blocks. Then `git add` and `git commit`.

### Prefer Target's version (ours)

```sh
git checkout --ours path/to/file
git add path/to/file
git commit
```

### Prefer Incoming version (theirs)

```sh
git checkout --theirs path/to/file
git add path/to/file
git commit
```

### If you picked wrong accidentally

```sh
git restore --staged path/to/file
git checkout -- path/to/file
```

> Then resolve conflicts again

### If the merge is a mess, abort

```sh
git merge --abort
```

## Rebase Path (often used to keep history linear)

Goal: replay your branch commits on top of another branch.

Example: put `feature-b` on top of `feature-a`

```sh
git checkout feature-b
git rebase feature-a
```

If a conflict appears:

```sh
git add <file>
git rebase --continue
```

If you need to cancel:

```sh
git rebase --abort
```

Since rebase rewrites history, pushing usually needs force:

```sh
git push --force-with-lease
```

> Use this to avoid clobbering others' work

## Habits to reduce conflicts

1. Pull from the targe branch before you start and before you push

```sh
git checkout main
git pull
git checkout your-feature
git merge main
```

Or, using rebase approach

```sh
git checkout your-feature
git pull --rebase origin main
```

2. Keep changes small. Smaller PRs merge faster and conflicts less.
3. Agree on code style and run a formatter. Fewer accidental formatting diffes means fewer conflicts.
4. **Communicate**. Ask the teammate who touched the same area to confirm the intended result.

## Troubleshooting quick list

- "I still see conflict markers after committing." You probably committed the markers. Reopen the file, remove markers, set final content, then `git add` and `git commit --amend`.
- "Git says everything up to date but my editor still shows conflict UI." Save all files. Run `git status`. If clean, the editor UI is stale. Reload the window.