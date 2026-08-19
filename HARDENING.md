<!-- markdownlint-disable -->

# Hardening Report: presubmit--ai-reviewer/v0.2.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **presubmit--ai-reviewer/v0.2.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ ... }} expression is interpolated directly inside a run: shell command string. In the 'Check required secrets' step, `${{ secrets.LLM_API_KEY }}` is embedded directly in the shell script: `if [ -z "${{ secrets.LLM_API_KEY }}" ]; then`. Any ${{ ... }} expression inside a run: block undergoes YAML template substitution before the shell sees it, making it a script-injection risk. The fix is to pass the secret via an env: variable and reference it as `"$LLM_API_KEY"` in the shell.

Locations:

- `.github/workflows/presubmit-review.yml:19`

### unpinned-uses (severity: high)

The workflow references `presubmit/ai-reviewer@latest`, which uses a mutable tag (`@latest`) instead of a pinned 40-character commit SHA. A mutable tag can be silently updated to point to a different (potentially malicious) commit, enabling supply-chain attacks. Pin the reference to a full SHA, e.g. `presubmit/ai-reviewer@<40-char-sha> # latest`.

Locations:

- `.github/workflows/presubmit-review.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed two findings in .github/workflows/presubmit-review.yml: (1) script-injection — moved `${{ secrets.LLM_API_KEY }}` from the run: shell string into an env: block on the 'Check required secrets' step, referencing it as `$LLM_API_KEY` in the shell; (2) unpinned-uses — pinned `presubmit/ai-reviewer@latest` to its full commit SHA `5f1290b6142b14b44cd2e8e3ffda84cd0a22e94f` with a `# latest` comment for readability.

