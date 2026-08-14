---
name: anthropic-marketplace-duplicate
description: Two separate official Anthropic Claude Code marketplaces both ship a plugin named frontend-design — installing from the wrong one creates a duplicate
metadata: 
  node_type: memory
  type: reference
  originSessionId: 4991d338-cd22-4149-a2b7-06f849465a86
  modified: 2026-07-27T12:25:36.051Z
---

Two distinct GitHub-backed marketplaces both contain a plugin literally named `frontend-design`:

- `claude-plugins-official` ← repo `anthropics/claude-plugins-official` — auto-installed by Claude Code itself (shows up even without the user asking), version reports as "unknown".
- `claude-code-plugins` ← repo `anthropics/claude-code` — the one actually requested via Slack ("/plugin marketplace add anthropics/claude-code" + "/plugin install frontend-design@claude-code-plugins"), resolves to a real semver (e.g. v1.1.0).

Confirmed 2026-07-27: installing `frontend-design@claude-plugins-official` on both Macs while the 2019 MacBook already had `frontend-design@claude-code-plugins` from the Slack bot produced a duplicate install (`claude plugin list` showed both). Cleaned up by uninstalling the `claude-plugins-official` copy on both machines and standardizing on `frontend-design@claude-code-plugins` everywhere — see the AIS-6 row in [[ai-skill-notion-db]].

**How to apply:** Before running `claude plugin install <name>@<marketplace>`, run `claude plugin list` first to check whether a same-named plugin is already installed under a *different* marketplace suffix. If the user references a specific marketplace/repo (e.g. from a Slack instruction or existing install), match that one rather than whichever marketplace happens to be auto-registered locally.
