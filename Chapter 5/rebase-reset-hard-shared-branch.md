# Why Are `git rebase` and `git reset --hard` Dangerous on a Shared Branch?

A **shared branch** is a Git branch that multiple developers use or depend on.

`git rebase` and `git reset --hard` can be dangerous on a shared branch because they can rewrite branch history or discard work that other people expect to remain available.

## 1. Why `git rebase` Can Be Dangerous

`git rebase` moves commits onto a new base and creates new commit IDs (SHAs) for the rebased commits.

### Risk

- Teammates may already have based their work on the original commits.
- Rewriting published history can make branches diverge.
- Teammates may need to reconcile their local history with the rewritten remote history.
- Force-pushing a rebased shared branch can overwrite or disrupt the remote branch history.

**Best practice:** Rebase local, unpublished commits when appropriate. Avoid rebasing commits that teammates have already based work on unless the team coordinates the change.

## 2. Why `git reset --hard` Can Be Dangerous

`git reset --hard <commit>` moves the current branch to the specified commit and resets the staging area and working directory to match it.

### Risk

- Later commits may no longer be reachable through that branch.
- Uncommitted changes in the working directory or staging area can be overwritten and lost.
- If the rewritten branch is pushed to a shared remote, teammates may see unexpected history changes and synchronization problems.

The commits are not necessarily deleted immediately; they may remain recoverable through another branch, a remote reference, or the reflog. However, recovery is not guaranteed indefinitely.

**Best practice:** Never use `git reset --hard` casually on a shared branch. Use a separate test branch for experiments. To undo a published commit, consider `git revert`, which creates a new commit that reverses the earlier change without rewriting existing history.

## 3. Example

Imagine the shared branch has this history:

```text
A --- B --- C   main
```

If someone runs:

```bash
git reset --hard A
```

the local branch moves back to `A`. Commits `B` and `C` are no longer part of that branch's history, even though they may still exist elsewhere in the repository.

If the rewritten history is then force-pushed, other collaborators may have to repair or reconcile their local branches.

## 4. Comparison

| Command | Main danger on a shared branch |
|---|---|
| `git rebase` | Rewrites commit history and changes commit SHAs. |
| `git reset --hard` | Moves the branch pointer and can discard staged or unstaged changes. |
| `git revert` | Creates a new commit that reverses earlier changes; usually safer for shared history. |

## Conclusion

`git rebase` can rewrite shared history, while `git reset --hard` can move a branch backward and discard local changes. On a shared branch, both can disrupt teammates and make synchronization or recovery difficult.

**Rule to remember:** Avoid rewriting published shared history. Coordinate with your team, use test branches for destructive experiments, and prefer `git revert` when undoing published commits.
