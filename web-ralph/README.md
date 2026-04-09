[中文说明](./README.zh-CN.md)

# Web Ralph

`web-ralph` is a Codex skill for Web development tasks that combines the `ralph` workflow from `oh-my-codex` with browser validation through `playwright-cli`, requiring browser-facing changes to pass both code-side checks and real browser-flow verification before the task can be considered complete.

It is built by combining two ideas:

- the `ralph` completion loop from `oh-my-codex`
- browser-side verification through `playwright-cli`

In plain terms: passing `test`, `build`, or `lint` is not enough. If the task affects what users see or do in the browser, the browser flow must also pass before the task can be treated as complete.

## What It Does

This skill extends a Ralph-style completion loop for web work by adding a hard browser-verification gate.

More specifically, it is a Web-focused extension of the `ralph` workflow in `oh-my-codex`, with `playwright-cli` used as the default browser validation tool.

It is designed for tasks such as:

- fixing UI bugs
- validating forms and navigation
- checking browser-visible state changes
- verifying responsive behavior
- catching console, runtime, API, or proxy failures that normal code checks can miss

## Core Rule

For browser-facing work, the task is not done unless:

- repo-side checks pass
- the target browser flow passes on a fresh run
- the expected visible result is present
- there are no related browser console or runtime errors
- there are no related failed API or proxy requests

If any of those fail, the loop stays open and the work continues.

## Why This Exists

Many web tasks look "done" from code alone but still fail in the browser.

Typical examples:

- a button renders but the request fails
- a route loads but throws console errors
- a form submits visually but the backend call is broken
- a page works on desktop but breaks on mobile

`web-ralph` makes those browser-level failures part of the completion criteria.

## Requirements

- `playwright-cli` available in the environment
- a runnable local app or reachable target URL
- a browser flow that can be reproduced and checked

## Directory Layout

```text
web-ralph/
  SKILL.md
  README.md
  README.zh-CN.md
  agents/
    openai.yaml
  references/
    browser-checklist.md
```

## Files

- `SKILL.md`: the actual skill instructions
- `agents/openai.yaml`: UI metadata for skill pickers
- `references/browser-checklist.md`: extra browser validation checklist

## Example Prompt

```text
Use $web-ralph to fix this login flow and keep going until the repo checks and browser flow both pass.
```

## Notes

- This skill is intentionally generic. It does not assume React, Vue, Vite, Next.js, or any specific repo layout.
- It is meant to be adapted by discovering the current project's start command, URL, and critical user flow before running the browser loop.
