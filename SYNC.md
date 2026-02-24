# Upstream Sync Guide

## Remotes

| Remote   | URL                                      | Purpose              |
|----------|------------------------------------------|----------------------|
| origin   | https://github.com/stavva-cardone/mcp   | Our fork (deploy)    |
| upstream | https://github.com/microsoft/mcp        | Microsoft source     |

## Branch Strategy

| Branch        | Purpose                                              |
|---------------|------------------------------------------------------|
| `main`        | Vendor mirror — tracks Microsoft closely, no edits   |
| `custom-main` | Our deployable branch — all customizations live here |

**We deploy from `custom-main`, never from `main`.**

Our changes go into `custom-main` directly, or into feature branches merged into `custom-main` via PR. `main` is never edited directly.

---

## Syncing with Upstream (run regularly)

### Step 1 — Update main from Microsoft

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

### Step 2 — Bring custom-main up to date

```bash
git checkout custom-main
git merge main
git push origin custom-main
```

Resolve any conflicts, then commit and push.

---

## Config Files

**Do not commit `mcp.json`** — it contains machine-specific absolute paths and is gitignored.

Setup for new team members:
```bash
cp mcp.json.example mcp.json
# Edit mcp.json and set the correct path for your machine
```

---

## Conflict Reduction

Preferred strategies in order:
1. Put our overrides in separate files that upstream doesn't touch.
2. Use a template + generate the real config at deploy time.
3. Use `.gitattributes` with `merge=ours` only for files where our version must always win.

---

## Rebase Policy

- `custom-main` stays **merge-based** (safe for team use, no history rewrites).
- Rebase is allowed on local feature branches before merging into `custom-main`.
