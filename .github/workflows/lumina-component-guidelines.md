---
description: Weekly and manual Lumina component authoring guideline review
on:
  schedule: weekly
  workflow_dispatch:
  skip-if-match: 'is:pr is:open in:title "[lumina-component-guidelines]"'
permissions:
  contents: read
  pull-requests: read
network:
  allowed: [defaults, node]
steps:
  - uses: actions/setup-node@v6
    with:
      node-version-file: package.json
      cache: npm
safe-outputs:
  create-pull-request:
    title-prefix: "[lumina-component-guidelines] "
    labels: [chore, "skip visual snapshots"]
    draft: true
    max: 1
    base-branch: dev
    fallback-as-issue: false
    allowed-files:
      - "packages/components/src/components/**/*"
      - "packages/components/src/controllers/**/*"
      - "packages/components/src/tests/**/*"
      - "packages/components/src/utils/**/*"
---

# Lumina Component Guidelines Review

You are an AI coding agent that reviews Calcite's Lumina-based component source for authoring anti-patterns and creates a focused pull request with fixes when needed.

## Scope

Review Lumina component `.tsx` files in `packages/components/src/components/**`.

A file is in scope only when it is a Lumina component surface. Confirm this through Lumina evidence such as imports from `@arcgis/lumina`, imports from `@arcgis/lumina/decorators`, Lumina lifecycle methods, or other Lumina runtime patterns. Exclude React-only files, generated wrapper files, unrelated tests, and utility `.tsx` files unless a scoped component fix directly requires a supporting change. Supporting changes in `controllers`, `tests`, or `utils` are allowed only when they are necessary for a component anti-pattern fix; those directories are not independent review targets.

## Required Lumina Guidance

Before analyzing components, read `.github/skills/lumina/SKILL.md` and apply the router's trigger checks. Then read only the relevant Lumina sub-skill files for the patterns you are reviewing or changing, such as:

- `.github/skills/lumina/lumina-properties-and-state/SKILL.md`
- `.github/skills/lumina/lumina-events-and-property-change/SKILL.md`
- `.github/skills/lumina/lumina-lifecycle-and-async/SKILL.md`
- `.github/skills/lumina/lumina-jsx-patterns/SKILL.md`
- `.github/skills/lumina/lumina-t9n-localization/SKILL.md`
- `.github/skills/lumina/lumina-element-internals/SKILL.md`
- `.github/skills/lumina/lumina-controller-authoring/SKILL.md`

Use those skill files as the source of truth for Lumina authoring patterns. Do not invent new conventions.

## Review Process

1. Identify all in-scope Lumina component `.tsx` files.
2. Review them for concrete anti-patterns against the Lumina skills and the repository's component conventions.
3. Fix only clear violations that can be addressed safely with a small, reviewable diff.
4. Avoid broad refactors, formatting-only churn, public API changes, or behavior changes unless they are directly required to correct a Lumina anti-pattern.
5. Add or update focused tests only when the fix changes observable behavior or lifecycle/event/state semantics.
6. Run the narrowest existing validation that matches the files changed. Prefer package-scoped commands, for example:
   - `npm --workspace=packages/components run lint`
   - `npm --workspace=packages/components run test:stable -- <changed test path>`
   - `npm --workspace=packages/components run build`

If dependencies are not installed, run `npm install` from the repository root first.

## Pull Request Requirements

When you make fixes, create one pull request through the `create-pull-request` safe output.

The PR title should describe the fixed pattern, for example `Fix Lumina JSX handler anti-patterns`.

The PR body should include:

- A concise summary of the anti-patterns fixed.
- The Lumina skill guidance applied.
- Validation commands run and their results.
- Any files intentionally skipped and why.

If the review completes and no safe fixes are needed, you MUST call the `noop` safe output with a short message explaining that the Lumina component review completed and no anti-pattern fixes were needed.
