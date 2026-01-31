# How to Target the Upstream Repository

This guide explains how to create a Pull Request (PR) from the fork `bachand/NautilusTelemetry` targeting the upstream repository `airbnb/NautilusTelemetry`.

## Current Setup

- **Fork (origin)**: `bachand/NautilusTelemetry` 
- **Upstream**: `airbnb/NautilusTelemetry`
- **Feature Branch**: `copilot/enforce-sendable-telemetry-attributes`
- **Target Branch**: `upstream/main`

## Git Remote Configuration

The repository is already configured with both remotes:

```bash
# View configured remotes
git remote -v

# Output:
# origin    https://github.com/bachand/NautilusTelemetry (fetch)
# origin    https://github.com/bachand/NautilusTelemetry (push)
# upstream  https://github.com/airbnb/NautilusTelemetry.git (fetch)
# upstream  https://github.com/airbnb/NautilusTelemetry.git (push)
```

## Current Branch Status

Your feature branch `copilot/enforce-sendable-telemetry-attributes` is:
- Based on `upstream/main` (commit bfa5459)
- 2 commits ahead of upstream/main
- Already pushed to the fork (`origin/copilot/enforce-sendable-telemetry-attributes`)

## Changes in This Branch

```
6 files changed, 22 insertions(+), 35 deletions(-)

- Sources/NautilusTelemetry/Tracing/Baggage.swift
- Sources/NautilusTelemetry/Tracing/Span.swift  
- Sources/NautilusTelemetry/Utilities/Identifiers.swift
- Tests/NautilusTelemetryTests/Tracing/BaggageTests.swift
- Tests/NautilusTelemetryTests/Tracing/SpanTests.swift
- Tests/NautilusTelemetryTests/Tracing/TracerTests.swift
```

## How to Create a PR Targeting Upstream

Since your branch is already pushed to your fork, you can create a PR in one of these ways:

### Option 1: GitHub Web UI (Recommended)

1. Navigate to your fork: https://github.com/bachand/NautilusTelemetry
2. Click "Pull requests" tab
3. Click "New pull request"
4. Click "compare across forks"
5. Set the following:
   - **base repository**: `airbnb/NautilusTelemetry`
   - **base**: `main`
   - **head repository**: `bachand/NautilusTelemetry`
   - **compare**: `copilot/enforce-sendable-telemetry-attributes`
6. Review the changes
7. Click "Create pull request"
8. Fill in the PR title and description
9. Click "Create pull request"

### Option 2: GitHub CLI

If you have GitHub CLI installed:

```bash
# From your feature branch
gh pr create --repo airbnb/NautilusTelemetry \
  --base main \
  --head bachand:copilot/enforce-sendable-telemetry-attributes \
  --title "Replace withLockUnchecked with compile-time Sendable enforcement" \
  --body "See commits for details"
```

### Option 3: Direct URL

Navigate directly to:
```
https://github.com/airbnb/NautilusTelemetry/compare/main...bachand:NautilusTelemetry:copilot/enforce-sendable-telemetry-attributes
```

## Verifying Your Changes

To see what will be included in the PR:

```bash
# View commit log
git log upstream/main..HEAD --oneline

# View file changes summary
git diff upstream/main..HEAD --stat

# View detailed diff
git diff upstream/main..HEAD
```

## Keeping Your Branch Updated

If upstream/main gets updated before your PR is merged:

```bash
# Fetch latest from upstream
git fetch upstream

# Rebase your branch on upstream/main (if needed)
git rebase upstream/main

# Force push to your fork (only if you rebased)
git push origin copilot/enforce-sendable-telemetry-attributes --force-with-lease
```

## Important Notes

1. **The PR is created on GitHub**, not locally via git commands
2. Your branch is already in the correct state and pushed to your fork
3. When creating the PR, make sure to select:
   - Base repository: `airbnb/NautilusTelemetry` 
   - Base branch: `main`
   - Head repository: `bachand/NautilusTelemetry`
   - Compare branch: `copilot/enforce-sendable-telemetry-attributes`

## PR Information

**Suggested Title:**
```
Replace withLockUnchecked with compile-time Sendable enforcement
```

**Summary of Changes:**
- Remove `withLockUnchecked` by enforcing `Sendable` at compile time
- Change `addAttribute` to use generic `<T: Hashable & Sendable>` constraint
- Make attribute subscripts get-only (remove setters)
- Update all test call sites to use `addAttribute` instead of subscript setters
- Update documentation to clarify behavior
```
