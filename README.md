# smallfire-ci-core

**Protected CI enforcement repository for smallfire WhatsApp MVP**

## Purpose

This repository contains the protected CI/CD workflow that enforces code quality standards for the smallfire project. By isolating the enforcement logic in a separate repo, we prevent bypassing quality gates during "urgent" fixes.

## Why This Exists

After experiencing multiple process violations where standards were bypassed under pressure, this governance model ensures:

1. **Immutable Standards**: CI rules cannot be changed in the same commit as code changes
2. **Separation of Concerns**: Enforcement logic is protected from emergency shortcuts
3. **Branch Protection**: Peekaboo can require the "enforce" check via GitHub branch protection

## What It Does

The `enforced-ci.yml` workflow runs:
- ✅ `ruff check .` - Blocking Python linting (no continue-on-error)
- ✅ `./test.sh --full` - Full test suite with pytest

## How It's Used

The main `smallfire-whatsapp-mvp` repository calls this workflow:

```yaml
jobs:
  enforce:
    uses: peekabo0jackson/smallfire-ci-core/.github/workflows/enforced-ci.yml@main
    secrets: inherit
```

## Branch Protection Setup

**Required by peekaboo:**
1. Go to repo Settings → Branches
2. Add rule for `main` branch
3. Require status checks: "enforce"
4. Require branches to be up to date
5. Include administrators (no exceptions)

This prevents any code from reaching production without passing the enforced standards.

## Maintenance

**IMPORTANT**: Changes to this repo should be rare and deliberate. Any modification to the enforcement rules must be:
- Discussed and approved by peekaboo
- Tested thoroughly
- Never made during an "urgent" fix

If you're tempted to change this repo under pressure - that's exactly when you shouldn't.

---

Created: 2025-10-17
Reason: Three process violations in one session proved structural enforcement needed
