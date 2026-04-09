[中文说明](./README.zh-CN.md)

# Web Ralplan

`web-ralplan` is a Codex skill for Web planning tasks. It extends the `ralplan` workflow with explicit browser-side test planning, so Web work is not scoped only around code checks.

It is built by combining:

- the consensus-planning workflow from `ralplan`
- Web-specific browser validation planning, with `playwright-cli` as the default browser verification tool

In plain terms: if a task changes what users see or do in the browser, the plan must define how that browser behavior will be verified before execution starts.

## What It Does

This skill is for the planning phase of Web work.

It makes sure the final plan and test spec do not stop at:

- unit tests
- lint
- build
- type checks

Instead, it forces the plan to also include:

- which page or route is affected
- which user flow must be tested
- what visible result proves success
- what browser-side failures block completion

## Why This Exists

A normal implementation plan can still be too code-centric for Web tasks.

That often leads to a bad pattern:

- planning only mentions repo-side checks
- browser validation gets improvised later during execution
- page bugs, console errors, failed requests, or broken mobile layouts are discovered too late

`web-ralplan` moves those browser requirements into the planning and test-spec stage.

## Core Rule

For browser-facing work, the final plan must define:

- the target page or route
- the critical browser flow
- the expected visible result
- the repo-side checks
- the browser-side checks
- the browser failure conditions

Failure conditions should explicitly include:

- relevant API request failures
- relevant local proxy failures
- related browser console errors
- related runtime errors

## Typical Handoff

The intended flow is:

```text
web-ralplan -> web-ralph
```

- `web-ralplan` writes the Web-aware plan and test spec
- `web-ralph` executes the work and closes the loop with real browser verification

## Requirements

- a Web task with browser-facing acceptance criteria
- enough repo or task context to identify routes, flows, and validation targets
- `playwright-cli` available if browser-tool choice needs to default to a concrete tool

## Directory Layout

```text
web-ralplan/
  SKILL.md
  README.md
  README.zh-CN.md
  agents/
    openai.yaml
  references/
    browser-test-plan.md
```

## Files

- `SKILL.md`: the actual planning skill instructions
- `agents/openai.yaml`: UI metadata for skill pickers
- `references/browser-test-plan.md`: reference for shaping browser-aware test specs

## Example Prompt

```text
Use $web-ralplan to create a Web implementation plan and test spec for this auth-flow change, including browser validation and failure conditions.
```

## Notes

- This skill is intentionally generic. It does not assume React, Vue, Next.js, Vite, or a fixed repo structure.
- It is meant to improve plan quality before execution, not to execute code directly.
