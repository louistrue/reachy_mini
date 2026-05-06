# Phase 0 handoff — getting the fork live at louistrue/clawson

This sandbox can't push outside `louistrue/reachy_mini`, so I prepared
the Phase-0 rebrand commit here as a patch for you to apply locally.

## What's in the patch

`0001-fork-rebrand.patch` — a single commit (~314 lines net) that:

- Retitles the README to "Clawson" + adds a fork notice linking
  upstream and `plan.md`.
- Updates `pyproject.toml` `[project.urls]` to point at
  `louistrue/clawson`, with a new `Upstream` field for `tomrikert/clawbody`.
- Drops `plan.md` (the canonical v1 plan) at the repo root.

Nothing functional changes. The Python package, entry-point command
(`clawbody`), and module path (`reachy_mini_openclaw`) all stay
identical so upstream pulls remain a clean merge.

## Apply on your laptop

Assumes you've already created `https://github.com/louistrue/clawson`
(empty or as a fork of `tomrikert/clawbody`).

```bash
# Clone your fork
git clone https://github.com/louistrue/clawson.git
cd clawson

# Add upstream so you can pull future clawbody changes
git remote add upstream https://github.com/tomrikert/clawbody.git
git fetch upstream

# If your fork is empty (created as a new repo, not via Fork button):
#   sync it with upstream first
#   git pull upstream main --allow-unrelated-histories
#   git push origin main

# Grab the patch from this repo's brainstorm branch
curl -L -o /tmp/0001-fork-rebrand.patch \
  https://raw.githubusercontent.com/louistrue/reachy_mini/claude/reachy-mini-app-ideas-Vd3lP/apps/clawson/handoff/0001-fork-rebrand.patch

# Apply (preserves the commit message + author)
git am /tmp/0001-fork-rebrand.patch

# Push
git push origin main
```

## What ships next (Phase 1)

After you confirm the fork boots end-to-end on your laptop with your
existing `.env` (OpenAI key + OpenClaw gateway), Phase 1 begins:
focus-mode state machine + antenna-press handler. No MCPs yet — the
goal of Phase 1 is just to get the deep / normal / available / snoozed
state correctly cycling and persisting across restarts.

I'll do that work in PRs against `louistrue/clawson` directly once
push access is sorted.

## Push access for future phases

Three options to unblock me pushing directly to `louistrue/clawson`
from a future Claude Code session:

1. **Add `louistrue/clawson` to this sandbox's allowlist** — easiest if
   you can configure the harness scope.
2. **Run me from your local machine** — `claude` CLI in your laptop's
   `~/clawson/` directory will use your real GitHub credentials.
3. **Patch handoff** — you keep applying patches like this one. Slowest
   but works without changes.
