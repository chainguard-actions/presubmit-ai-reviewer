<!-- markdownlint-disable -->

# Hardening Report: presubmit--ai-reviewer/v0.2.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **presubmit--ai-reviewer/v0.2.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: The `run:` block in the 'Check required secrets' step directly interpolates a `${{ secrets.LLM_API_KEY }}` expression inside the shell command string (`if [ -z "${{ secrets.LLM_API_KEY }}" ]`). Any `${{ ... }}` expression is substituted by the Actions runner before the shell ever sees the command, meaning the value is embedded raw into the shell script. Even for secrets, this pattern is unsafe — the value should be passed via an `env:` variable and referenced as `$ENV_VAR` inside the script instead.

Locations:

- `.github/workflows/presubmit-review.yml:19`

### unpinned-uses (severity: high)

The workflow references `presubmit/ai-reviewer@latest`, which is a mutable tag. If the upstream repository is compromised or the tag is moved, the workflow will silently execute arbitrary new code. This should be pinned to a full 40-character commit SHA (e.g., `presubmit/ai-reviewer@<sha> # latest`) to prevent supply-chain attacks.

Locations:

- `.github/workflows/presubmit-review.yml:23`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in .github/workflows/presubmit-review.yml: (1) script-injection: moved `${{ secrets.LLM_API_KEY }}` from the `run:` shell string into an `env:` block and referenced it as `$LLM_API_KEY` in the shell script; (2) unpinned-uses: pinned `presubmit/ai-reviewer@latest` to the full commit SHA `5f1290b6142b14b44cd2e8e3ffda84cd0a22e94f` with a `# latest` comment.

