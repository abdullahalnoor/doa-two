# Git: Difference Between `fetch + merge` and `pull`

## The main difference

**`git fetch` + `git merge`** lets you download remote changes first and decide when to integrate them.

**`git pull`** downloads remote changes and integrates them into your current branch in one command.

By default, `git pull` generally uses merge, but it can be configured to rebase instead.

## 1. Fetch + Merge

Typical workflow:

```bash
git fetch origin
git log --oneline main..origin/main
git merge origin/main
```

What each command does:

1. `git fetch origin` downloads new commits and updates remote-tracking references, such as `origin/main`. It does **not** merge those changes into your local `main`.
2. `git log --oneline main..origin/main` shows commits reachable from `origin/main` that are not yet in your local `main`, so you can review them.
3. `git merge origin/main` integrates the fetched changes into the current branch. Make sure you are on `main` first if that is the branch you want to update.

### Flow

```text
Remote repository (origin/main)
             |
             | git fetch origin
             v
Updated remote-tracking reference
             |
             | Review incoming commits
             v
             | git merge origin/main
             v
Updated local main branch
```

## 2. Pull

Typical workflow:

```bash
git checkout main
git pull
```

`git pull` fetches from the configured upstream and then integrates the changes into the current branch. The integration method is usually **merge**, unless your Git configuration or command options specify rebase or another strategy.

### Flow

```text
Remote repository
        |
        | git pull
        v
Fetch + integrate
        |
        v
Updated local branch
```

## 3. Comparison table

| Command | What it does | Lets you review before integrating? |
|---|---|---|
| `git fetch origin` | Downloads remote updates without integrating them into your current branch | Yes |
| `git merge origin/main` | Integrates the fetched `origin/main` changes into the current branch | You choose when to run it |
| `git pull` | Fetches and integrates remote changes | Not as a separate step by default |

## 4. Example: Nanu's Recipe Book

Suppose Mitu adds `kala-bhuna.md` and pushes it to GitHub.

### Option A: Fetch, inspect, then merge

```bash
git checkout main
git fetch origin
git log --oneline main..origin/main
git merge origin/main
```

You download Mitu's commits, inspect the incoming commits, and then merge them into your local `main`.

### Option B: Pull directly

```bash
git checkout main
git pull
```

Git fetches and integrates the upstream changes in one command. After a successful update, Mitu's committed recipe should be available in your local working tree.

## 5. Which should you use?

- **Use `fetch` + `merge`** when you want to inspect incoming commits before integrating them.
- **Use `pull`** when you want to synchronize your local branch in one step.

## Easy way to remember

- `git fetch` = **download**
- `git merge` = **integrate**
- `git pull` = **download + integrate**

**Important:** `git pull` does not always create a merge commit. If configured to rebase, it rebases your local commits on top of the fetched remote commits instead.
