# web-skills-for-codex-claude_code

[中文说明](./README.zh-CN.md)

![skills](https://img.shields.io/badge/skills-2-blue)
![workflow](https://img.shields.io/badge/workflow-deep--interview%20%E2%86%92%20web--ralplan%20%E2%86%92%20web--ralph-6f42c1)
![oh-my-codex](https://img.shields.io/badge/oh--my--codex-compatible-111827)
![playwright-cli](https://img.shields.io/badge/browser_validation-playwright--cli-2ea44f)
![license](https://img.shields.io/badge/license-MIT-yellow)

Skill docs:

- `web-ralplan`: [EN](./web-ralplan/README.md) | [中文](./web-ralplan/README.zh-CN.md)
- `web-ralph`: [EN](./web-ralph/README.md) | [中文](./web-ralph/README.zh-CN.md)

This repository contains a small set of Web-focused Codex skills.

It is meant to solve two practical problems:

- define browser-aware plans and test specs before coding starts
- require real browser verification before browser-facing work can be treated as complete

The repository currently includes two skills:

- `web-ralplan`: for the planning stage, so Web flows, browser checks, and failure conditions are written into the plan and test spec
- `web-ralph`: for the execution stage, so browser-facing work must pass real browser verification before completion

There is also one workflow rule:

- in the `oh-my-codex` workflow, `deep-interview` should come first

Recommended sequence:

```text
deep-interview -> web-ralplan -> web-ralph
```

## Before You Install

Make sure you already have:

1. Codex CLI
2. `oh-my-codex`
3. `node` and `npm`

If `oh-my-codex` is correctly installed, this command should work:

```bash
omx setup --force --verbose
```

If `omx` is not available, install or fix `oh-my-codex` first.

## One-Shot Install

From inside this repository directory, paste:

```bash
omx setup --force --verbose && \
npm install -g @playwright/cli@latest && \
mkdir -p "${HOME}/.codex/skills" && \
rm -rf "${HOME}/.codex/skills/web-ralplan" "${HOME}/.codex/skills/web-ralph" && \
cp -R ./web-ralplan "${HOME}/.codex/skills/web-ralplan" && \
cp -R ./web-ralph "${HOME}/.codex/skills/web-ralph"
```

This does three things:

1. runs `oh-my-codex` setup
2. installs `playwright-cli`
3. copies both skills into `~/.codex/skills`

## Verify Installation

Paste this next:

```bash
omx doctor && \
playwright-cli --version && \
ls "${HOME}/.codex/skills/web-ralplan" "${HOME}/.codex/skills/web-ralph"
```

If those commands succeed, installation is complete.

## Start Using

### Step 1: clarify the task

```text
$deep-interview "clarify this web task before planning and execution"
```

Use this to clarify scope, goals, non-goals, and decision boundaries.

### Step 2: create the Web-aware plan

```text
$web-ralplan "plan this web task with browser validation"
```

This writes the Web testing side into the plan:

- which pages or routes are affected
- which user flows must be tested
- what visible result means success
- what browser-side failures count as blockers

### Step 3: execute and verify in the browser

```text
$web-ralph "finish this web task and verify it in the browser"
```

This requires:

- repo-side verification to pass
- browser flows to pass
- no related console/runtime/API/proxy failures

## Repository Layout

```text
codex-web-skills/
  README.md
  README.zh-CN.md
  web-ralplan/
    SKILL.md
    README.md
    README.zh-CN.md
    agents/
      openai.yaml
    references/
      browser-test-plan.md
  web-ralph/
    SKILL.md
    README.md
    README.zh-CN.md
    agents/
      openai.yaml
    references/
      browser-checklist.md
```

## Summary

If you want the shortest path:

1. enter this directory
2. run the one-shot install command
3. run the verification command
4. use `deep-interview`
5. use `web-ralplan`
6. use `web-ralph`
